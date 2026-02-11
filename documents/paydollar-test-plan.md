---
title: "PayDollar Test Cards"
date: 2026-02-06
---
# PayDollar Test Cards

Reference: https://developer.asiapay.com/integration-guide/appendix-a/3ds-testing-cards

> To test for payment failure case, use a different Expiry Date and/or Secure Code.
> Remember online refund at the PayDollar console!!!

## Common Test Info
```
Email: test@example.com
Phone: +852 91234567
```

## Cards for 3DS 2.2 (Challenge Flow)

### Visa
```
Card:   4333880000200027
Expiry: 07/2030
CVV:    123
OTP:    123456
```
- [x] Desktop browser
- [ ] iPhone
- [x] Android

### Mastercard
```
Card:   5422880000200026
Expiry: 07/2030
CVV:    123
OTP:    123456
```
- [x] Desktop browser
- [ ] iPhone
- [x] Android

### AMEX
```
Card:   371891000200025
Expiry: 07/2030
CVV:    1234
OTP:    123456
```
- [x] Desktop browser
- [ ] iPhone
- [x] Android

### UnionPay (CUP)
```
Card:   6250947000000014
Expiry: 12/2033
CVV:    123
OTP:    111111 (Desktop) / 123456 (Mobile)
```
- [x] Desktop browser
- [ ] iPhone
- [x] Android

### Discover
```
Card:   36189141071950
Expiry: 07/2030
CVV:    123
```
- [x] Desktop browser
- [ ] iPhone
- [x] Android

## PayMe (No wallet)
```
Success: amount ends .81 (e.g., 100.81)
Failure: amount ends .77 (e.g., 100.77)
```
- [x] Desktop browser
- [ ] iPhone
- [x] Android

## Alipay HK (use real wallet)
- [x] Desktop browser
- [ ] iPhone
- [x] Android
