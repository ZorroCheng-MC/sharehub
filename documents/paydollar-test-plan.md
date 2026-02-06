---
title: "PayDollar Test Cards"
date: 2026-02-06
---
# PayDollar Test Cards

Reference: https://developer.asiapay.com/integration-guide/appendix-a/3ds-testing-cards

> To test for payment failure case, use a different Expiry Date and/or Secure Code.

## Common Test Info
```
Email: test@example.com
Phone: 91234567
```

## Cards for 3DS 2.2 (Challenge Flow)

### Visa
```
Card:   4333880000200027
Expiry: 07/2030
CVV:    123
OTP:    123456
```
- [ ] Desktop browser
- [ ] iPhone
- [ ] Android

### Mastercard
```
Card:   5422880000200026
Expiry: 07/2030
CVV:    123
OTP:    123456
```
- [x] Desktop browser
- [x] iPhone
- [ ] Android

### AMEX
```
Card:   371891000200025
Expiry: 07/2030
CVV:    1234
OTP:    123456
```
- [ ] Desktop browser
- [x] iPhone
- [ ] Android

### UnionPay (CUP)
```
Card:   6220230626000073
Expiry: 07/2030
CVV:    123
OTP:    123456
```
- [x] Desktop browser
- [ ] iPhone
- [ ] Android

### Discover
```
Card:   6011000300002585
Expiry: 07/2030
CVV:    123
OTP:    123456
```
- [x] Desktop browser
- [x] iPhone
- [ ] Android

### JCB
```
Card:   3518910000200021
Expiry: 07/2030
CVV:    123
OTP:    123456
```
- [ ] Desktop browser
- [ ] iPhone
- [ ] Android

## Cards for 3DS 2.2 (Frictionless Flow)

### Visa
```
Card:   4333880000200019
Expiry: 07/2030
CVV:    123
```

### Mastercard
```
Card:   5422880000200018
Expiry: 07/2030
CVV:    123
```

### AMEX
```
Card:   371891000200017
Expiry: 07/2030
CVV:    1234
```

### UnionPay (CUP)
```
Card:   6220230626000057
Expiry: 07/2030
CVV:    1234
```

### Discover
```
Card:   6011000300002569
Expiry: 07/2030
CVV:    1234
```

### JCB
```
Card:   3518910000200013
Expiry: 07/2030
CVV:    123
```

## Cards for No 3DS

### Visa
```
Card:   4918914107195005
Expiry: 07/2030
CVV:    123
```

### Mastercard
```
Card:   5258950000000020
Expiry: 07/2030
CVV:    123
```

### AMEX
```
Card:   378416000100026
Expiry: 07/2030
CVV:    1234
```

### JCB
```
Card:   3528010000010021
Expiry: 07/2030
CVV:    123
```

## PayMe (No wallet)
```
Success: amount ends .81 (e.g., 100.81)
Failure: amount ends .77 (e.g., 100.77)
```
- [x] Desktop browser
- [x] iPhone
- [ ] Android

## Alipay HK (Real wallet, amount 0.10)
- [ ] Desktop browser
- [ ] iPhone
- [ ] Android
