# 🏨 Hotel Reservation System

## 📋 Phase 1 Report: Adding Two New Features

## ✨ Added Features

### 1️⃣ 📱 SMS Sending Feature

#### Changes Made:
- 🔄 **Renamed the method in the MessageSender interface to `sendMessage`**
- 🔄 **Renamed the method in the EmailSender class to match the interface**
- ➕ **Added a new class `SmsSender` in the `service` package**
- 🔧 **Updated the ReservationService class**:
  - Added a new `case` for dynamically selecting the message sending method

---

### 2️⃣ 💳 New Payment Service

#### Changes Made:
- ➕ **Added a new on-site payment method (`payOnSite`) in PaymentProcessor**
- 🔧 **Added a new `case` in ReservationService to support the new payment type**

---

## 📋 Phase 2 Report: SOLID Principles


## 🟦 SRP — Single Responsibility Principle

### ❌ Violations
- **ReservationService**
  - Handles multiple unrelated responsibilities:
    - reservation processing
    - applying discounts
    - managing payments
    - printing invoices
    - sending notifications

- **PaymentProcessor**
  - Contains three payment methods in a single class:
    - payByCard
    - payByCash
    - payByPayPal

- **Reservation**
  - Stores reservation data AND calculates the price
  - Price calculation should be moved to a separate service

### ✅ Correct Usage
- **EmailSender** → only responsible for sending emails
- **Customer** and **Room** → data-holder classes
- **LuxuryRoom** → only adds luxury features

---

## 🟩 OCP — Open–Closed Principle

### ❌ Violations
- **ReservationService**
  - Uses switch-case for payment methods
  - Uses switch-case for notification types
  - Adding a new type requires modifying existing code

- **PaymentProcessor**
  - Must be changed whenever a new payment method is introduced

- **Main**
  - Depends directly on concrete classes
  - Cannot replace implementations without modifying code

### ✅ Correct Usage
- **LuxuryRoom** → extends Room without modifying parent
- **PaymentMethods enum** → can accept new values

---

## 🟨 LSP — Liskov Substitution Principle

### ❌ Violations
None found.

### ✅ Correct Usage
- **LuxuryRoom extends Room**
  - Does not override any methods
  - Does not change parent behavior
  - Fully substitutable for Room

- **EmailSender implements MessageSender**
  - Fully implements interface contract

---

## 🟧 ISP — Interface Segregation Principle

### ❌ Violations
- **MessageSender interface**
  - Name is generic but only provides sendEmail()
  - Adding SMS or other channels would require modifying the interface

- **PaymentProcessor**
  - If it had an interface, it would become a fat interface with multiple unrelated responsibilities

### ✅ Correct Usage
- **MessageSender**
  - Small and focused

---

## 🟥 DIP — Dependency Inversion Principle

### ❌ Violations
- **ReservationService**
  - Directly depends on PaymentProcessor (instantiates it)
  - Directly depends on EmailSender

- **Main**
  - Creates all concrete classes directly

- **Reservation**
  - Has public fields referencing concrete classes

- **Entire system**
  - Does not depend on abstractions at all

### ✅ Correct Usage
None.

---

## 🟪 PLK — Law of Demeter

### ❌ Violations
- **ReservationService**
  - Chained access:
    - res.customer.city
    - res.customer.email
    - res.room.price
  - Direct modification:
    - res.room.price *= 0.9

- **Reservation**
  - Direct access to room.price

### ✅ Correct Usage
- **EmailSender**
  - Works with simple parameters
- **PaymentProcessor**
  - Works with simple parameters

---

## 🟫 CRP — Composition Over Inheritance

### ❌ Violations
- **LuxuryRoom**
  - Inherits from Room rather than using composition
  - Should have *a Room* + luxury features

### ✅ Correct Usage
- **Reservation**
  - Uses composition for Room and Customer

- **ReservationService**
  - Uses composition for PaymentProcessor
