---
date: 2026-01-09
status: active
project: ccf-shave-for-hope
type: technical-guide
audience: admin-dashboard-dev-team
related: "[[2026-01-06-ccf-shave-for-hope-technical-plan]]"
---

# Shave for Hope - Firebase Admin Dashboard Guide

> Technical guide for the admin dashboard development team to understand and interact with the Firebase backend.

---

## Firebase Project Info

| Item | Value |
|------|-------|
| **Project ID** | `studio-3022188308-abeaa` |
| **Region** | `asia-east1` (Hong Kong) |
| **Console URL** | https://console.firebase.google.com/project/studio-3022188308-abeaa |
| **Firestore** | https://console.firebase.google.com/project/studio-3022188308-abeaa/firestore |
| **Storage** | https://console.firebase.google.com/project/studio-3022188308-abeaa/storage |
| **Auth** | https://console.firebase.google.com/project/studio-3022188308-abeaa/authentication |

---

## Firestore Collections

### 1. `users` - Registered Users

```typescript
interface UserProfile {
  uid: string;                    // Firebase Auth UID (document ID)
  displayName: string;            // Required
  email: string;                  // From OAuth/registration
  avatarUrl?: string;             // Profile photo URL
  instagramHandle?: string;       // e.g., "@username"
  instagramPostUrl?: string;      // User's shared IG post
  fundraisingGoal?: number;       // HKD target
  personalStory?: string;         // Why they support CCF
  language: 'zh' | 'en';          // UI language preference
  createdAt: Timestamp;
  updatedAt: Timestamp;

  // Usage quotas (nested objects)
  imageQuota?: {
    dailyLimit: number;           // Default: 4
    weeklyLimit: number;          // Default: 10
    lifetimeLimit: number;        // Default: 20
    dailyUsed: number;
    weeklyUsed: number;
    lifetimeUsed: number;
    dailyResetAt: Timestamp;      // HKT midnight
    weeklyResetAt: Timestamp;     // Monday HKT midnight
    unlimited?: boolean;          // Admin override
  };
  videoQuota?: {
    // Same structure, defaults: 2/5/10
  };
}
```

**Admin Dashboard Use Cases:**
- View all registered users
- Search users by email/displayName
- Modify user quotas (set `unlimited: true` for VIP users)
- View user activity statistics

**Example Queries:**
```javascript
// Get all users
const users = await getDocs(collection(db, 'users'));

// Get user by UID
const user = await getDoc(doc(db, 'users', uid));

// Search by email
const q = query(collection(db, 'users'), where('email', '==', email));

// Set unlimited quota for a user
await updateDoc(doc(db, 'users', uid), {
  'imageQuota.unlimited': true,
  'videoQuota.unlimited': true
});
```

---

### 2. `transformations` - Registered User Transformations

```typescript
interface Transformation {
  id: string;                     // Auto-generated document ID
  userId: string;                 // Reference to users collection
  originalImageUrl: string;       // Firebase Storage URL
  transformedImageUrl: string;    // Firebase Storage URL
  videoUrl?: string;              // If video was generated
  videoStatus?: 'pending' | 'processing' | 'complete' | 'failed';
  videoLanguage?: 'zh' | 'en';
  shareCount: number;             // Times shared
  isPublic: boolean;              // Visible on public profile
  createdAt: Timestamp;
}
```

**Admin Dashboard Use Cases:**
- View all transformations with user info
- Filter by date range
- View transformation statistics
- Moderate inappropriate content

**Example Queries:**
```javascript
// Get recent transformations
const q = query(
  collection(db, 'transformations'),
  orderBy('createdAt', 'desc'),
  limit(50)
);

// Get transformations by user
const q = query(
  collection(db, 'transformations'),
  where('userId', '==', uid)
);

// Count total transformations
const snapshot = await getCountFromServer(collection(db, 'transformations'));
console.log(snapshot.data().count);
```

---

### 3. `anonymousTransformations` - Guest User Transformations

```typescript
interface AnonymousTransformation {
  id: string;
  visitorId: string;              // Browser UUID
  originalImagePath: string;      // Storage path
  transformedImagePath: string;   // Storage path
  originalImageUrl: string;       // Download URL
  transformedImageUrl: string;    // Download URL
  shareId?: string;               // Link to shares collection
  createdAt: Timestamp;
  expiresAt: Timestamp;           // 7 days from creation (for DLC)
}
```

**Note:** These are automatically cleaned up after 90 days via Storage lifecycle rules.

---

### 4. `anonymousQuotas` - Guest User Rate Limiting

```typescript
interface AnonymousQuota {
  visitorId: string;              // Browser UUID (document ID)
  dailyCount: number;             // Today's transforms (max 3)
  dailyResetAt: Timestamp;        // Next HKT midnight
  lifetimeCount: number;          // Total transforms ever
  createdAt: Timestamp;
  updatedAt: Timestamp;
}
```

**Admin Dashboard Use Cases:**
- Monitor anonymous usage patterns
- Identify potential abuse (high lifetime counts)
- Reset quotas if needed

---

### 5. `shares` - Public Share Pages

```typescript
interface Share {
  id: string;                     // Short ID used in URL
  imageUrl: string;               // Full image (~300KB)
  ogImageUrl?: string;            // OG thumbnail (1200x630, ~150KB)
  userId?: string;                // If logged in
  visitorId?: string;             // If anonymous
  platform?: string;              // whatsapp, facebook, instagram, copy
  createdAt: Timestamp;
}
```

**Share Page URL:** `https://shaveforhope.ccf.org.hk/share/{id}`

**Admin Dashboard Use Cases:**
- View all shares with platform breakdown
- Track viral shares (by view count if implemented)
- Moderate shared content

---

### 6. `instagramPosts` - Community Gallery

```typescript
interface InstagramPost {
  id: string;
  postUrl: string;                // https://instagram.com/p/ABC123
  igUsername: string;             // Lowercase IG handle
  mediaUrl: string;               // Firebase Storage URL (not IG CDN)
  mediaType: 'image' | 'video';
  caption?: string;
  postedAt: Timestamp;            // When posted on IG
  harvestedAt: Timestamp;         // When added to our system
  matchedUserId?: string;         // Our user ID if matched
  source: 'scraper' | 'manual';
}
```

**Admin Dashboard Use Cases:**
- Add/remove posts from gallery
- Match posts to registered users
- Moderate inappropriate content

**Example: Add a post manually:**
```javascript
await addDoc(collection(db, 'instagramPosts'), {
  postUrl: 'https://instagram.com/p/ABC123',
  igUsername: 'username',
  mediaUrl: 'https://storage.googleapis.com/...',
  mediaType: 'image',
  caption: 'My shave for hope!',
  postedAt: Timestamp.now(),
  harvestedAt: Timestamp.now(),
  source: 'manual'
});
```

---

### 7. `statistics` - Global Metrics (Single Document)

```typescript
// Document ID: 'global'
interface Statistics {
  totalUsers: number;
  totalImagesAnonymous: number;
  totalImagesLoggedIn: number;
  totalVideos: number;
  totalShares: number;
  sharesByPlatform: {
    whatsapp: number;
    facebook: number;
    instagram: number;
    copy: number;
  };
  updatedAt: Timestamp;
}
```

**Admin Dashboard Use Cases:**
- Display on admin dashboard homepage
- Generate reports

**Example: Read statistics:**
```javascript
const statsDoc = await getDoc(doc(db, 'statistics', 'global'));
const stats = statsDoc.data();
console.log(`Total users: ${stats.totalUsers}`);
```

---

## Firebase Storage Structure

```
Firebase Storage Bucket
├── /transformations/{userId}/
│   ├── original_{timestamp}.jpg      (~500KB)
│   └── transformed_{timestamp}.jpg   (~300KB)
│
├── /anonymous/{visitorId}/
│   ├── original_{timestamp}.jpg      (~500KB, 90-day retention)
│   └── transformed_{timestamp}.jpg   (~300KB, 90-day retention)
│
├── /public/shares/
│   ├── {timestamp}_{id}.jpg          (~300KB, permanent)
│   └── og_{timestamp}_{id}.jpg       (~150KB, OG thumbnail)
│
├── /videos/{userId}/
│   └── video_{timestamp}.mp4         (~10-30MB)
│
├── /avatars/{userId}/
│   └── avatar.jpg                    (~200KB)
│
└── /archive/                         (Read-only, managed by DLC)
```

**Storage URLs format:**
```
https://firebasestorage.googleapis.com/v0/b/studio-3022188308-abeaa.firebasestorage.app/o/{path}?alt=media
```

---

## Security Rules Summary

### Firestore Rules

| Collection | Read | Write |
|------------|------|-------|
| `users` | Owner only | Owner only |
| `transformations` | Public if `isPublic`, else owner | Owner |
| `anonymousTransformations` | Public | Create only |
| `anonymousQuotas` | Public | Public |
| `shares` | Public | Create only |
| `instagramPosts` | Public | Admin SDK only |
| `statistics` | Public | Public (increment) |

### Storage Rules

| Path | Read | Write |
|------|------|-------|
| `/transformations/{userId}/` | Public | Owner, <2MB |
| `/anonymous/{visitorId}/` | Public | Anyone, <2MB |
| `/public/shares/` | Public | Anyone, <1MB |
| `/videos/{userId}/` | Public | Owner, <50MB |
| `/avatars/{userId}/` | Public | Owner, <1MB |
| `/archive/` | Public | None (DLC only) |

---

## Admin SDK Setup

For the admin dashboard, use Firebase Admin SDK (server-side) to bypass security rules:

```javascript
// Initialize Admin SDK
const admin = require('firebase-admin');
const serviceAccount = require('./service-account-key.json');

admin.initializeApp({
  credential: admin.credential.cert(serviceAccount),
  storageBucket: 'studio-3022188308-abeaa.firebasestorage.app'
});

const db = admin.firestore();
const storage = admin.storage();

// Now you can read/write any collection
const users = await db.collection('users').get();
```

**Get Service Account Key:**
1. Go to Firebase Console → Project Settings → Service Accounts
2. Click "Generate new private key"
3. Store securely (never commit to git!)

---

## Common Admin Operations

### 1. Get Dashboard Statistics

```javascript
async function getDashboardStats() {
  const [stats, usersCount, transformsCount, sharesCount] = await Promise.all([
    db.collection('statistics').doc('global').get(),
    db.collection('users').count().get(),
    db.collection('transformations').count().get(),
    db.collection('shares').count().get(),
  ]);

  return {
    ...stats.data(),
    verifiedUsersCount: usersCount.data().count,
    verifiedTransformsCount: transformsCount.data().count,
    verifiedSharesCount: sharesCount.data().count,
  };
}
```

### 2. Search Users

```javascript
async function searchUsers(searchTerm) {
  // Note: Firestore doesn't support full-text search
  // For production, consider Algolia or Elasticsearch

  // Search by exact email
  const byEmail = await db.collection('users')
    .where('email', '==', searchTerm)
    .get();

  // For partial match, fetch all and filter client-side
  // (not recommended for large datasets)
}
```

### 3. Grant Unlimited Quota

```javascript
async function grantUnlimitedQuota(uid) {
  await db.collection('users').doc(uid).update({
    'imageQuota.unlimited': true,
    'videoQuota.unlimited': true,
  });
}
```

### 4. Reset Anonymous Quota

```javascript
async function resetAnonymousQuota(visitorId) {
  await db.collection('anonymousQuotas').doc(visitorId).delete();
}

// Reset all anonymous quotas
async function resetAllAnonymousQuotas() {
  const batch = db.batch();
  const quotas = await db.collection('anonymousQuotas').get();
  quotas.docs.forEach(doc => batch.delete(doc.ref));
  await batch.commit();
}
```

### 5. Add Instagram Post to Gallery

```javascript
async function addInstagramPost(postUrl, mediaUrl, username) {
  await db.collection('instagramPosts').add({
    postUrl,
    mediaUrl,
    igUsername: username.toLowerCase(),
    mediaType: 'image',
    postedAt: admin.firestore.Timestamp.now(),
    harvestedAt: admin.firestore.Timestamp.now(),
    source: 'manual',
  });
}
```

### 6. Export Data for Reports

```javascript
async function exportTransformationReport(startDate, endDate) {
  const transforms = await db.collection('transformations')
    .where('createdAt', '>=', startDate)
    .where('createdAt', '<=', endDate)
    .orderBy('createdAt', 'desc')
    .get();

  return transforms.docs.map(doc => ({
    id: doc.id,
    ...doc.data(),
    createdAt: doc.data().createdAt.toDate().toISOString(),
  }));
}
```

---

## Environment Variables

For admin dashboard, you'll need:

```env
# Firebase Admin SDK
FIREBASE_PROJECT_ID=studio-3022188308-abeaa
FIREBASE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\n..."
FIREBASE_CLIENT_EMAIL=firebase-adminsdk-xxxxx@studio-3022188308-abeaa.iam.gserviceaccount.com

# Storage bucket
FIREBASE_STORAGE_BUCKET=studio-3022188308-abeaa.firebasestorage.app
```

---

## Data Lifecycle & Cleanup

| Data Type | Retention | Cleanup Method |
|-----------|-----------|----------------|
| Anonymous images | 90 days | Storage DLC (auto) |
| Anonymous quotas | Indefinite | Manual or cron |
| Registered user data | Indefinite | Manual on request |
| Share pages | Indefinite | Manual moderation |
| Statistics | Indefinite | N/A |

---

## Useful Links

- [Firebase Console](https://console.firebase.google.com/project/studio-3022188308-abeaa)
- [Firestore Documentation](https://firebase.google.com/docs/firestore)
- [Admin SDK Setup](https://firebase.google.com/docs/admin/setup)
- [Technical Plan](./2026-01-06-ccf-shave-for-hope-technical-plan.md)
- [Master Project Plan](./2025-12-31-ccf-shave-for-hope-firebase-studio-project.md)

---

*Document Version: 1.0 | Created: 2026-01-09 | Author: Development Team*
