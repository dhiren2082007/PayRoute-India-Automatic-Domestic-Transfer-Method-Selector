# PayRoute-India-Automatic-Domestic-Transfer-Method-Selector
# 🇮🇳 IndiaPaymentRouter

**IndiaPaymentRouter** is a Spring Boot application that automatically routes domestic Indian bank transfers to the appropriate payment rail (**UPI, IMPS, or NEFT**) based on the transfer amount.

The application provides both:

- 🌐 A REST API for programmatic transfers
- 💻 An interactive command-line console for manual testing and viewing transfer history

---

## 🚀 Features

- ✅ Automatic payment routing based on transfer amount
- ✅ REST API built using Spring Boot
- ✅ Interactive CLI console for manual transfers
- ✅ Input validation for sender and receiver details
- ✅ Business rule validation
- ✅ Centralized exception handling
- ✅ Structured JSON error responses
- ✅ In-session transfer history

---

## 💳 Payment Routing Logic

| Transfer Amount | Payment Method |
|-----------------|----------------|
| ≤ ₹1,00,000 | UPI |
| ≤ ₹2,00,000 | IMPS |
| > ₹2,00,000 | NEFT |

---

## 🛠 Tech Stack

- Java 21
- Spring Boot 3.5.4
- Spring Web
- Maven
- REST API

---

## 📂 Project Structure

```text
IndiaPaymentRouter/
├── src/main/java/com/paymentrouter/
│   ├── IndiaPaymentRouterApplication.java
│   ├── controller/
│   │   └── TransferController.java
│   ├── service/
│   │   └── TransferService.java
│   ├── dto/
│   │   ├── TransferRequest.java
│   │   ├── ReceiverDetails.java
│   │   └── TransferResponse.java
│   └── exception/
│       ├── GlobalExceptionHandler.java
│       ├── ErrorResponse.java
│       ├── InvalidSenderDetailsException.java
│       ├── InvalidReceiverDetailsException.java
│       ├── InvalidAmountException.java
│       └── SameAccountTransferException.java
├── src/main/resources/
│   └── application.properties
└── pom.xml
```

---

# ⚙️ Prerequisites

- Java JDK 21 or later
- Maven

---

# ▶️ Running the Application

Clone the repository

```bash
git clone https://github.com/your-username/IndiaPaymentRouter.git
```

Navigate to the project

```bash
cd IndiaPaymentRouter
```

Run the application

```bash
./mvnw spring-boot:run
```

or

```bash
mvn spring-boot:run
```

The application starts on **http://localhost:8082**

Both the REST API and the interactive console run simultaneously.

---

# 🌐 REST API

## POST `/api/transfer`

### Request

```json
{
  "sender_name": "John Doe",
  "acc_no": "1234567890",
  "bank_name": "HDFC Bank",
  "amount": 50000,
  "receiver": {
    "receiver_name": "Jane Smith",
    "acc_no": "0987654321",
    "bank_name": "ICICI Bank"
  }
}
```

---

## Successful Response

```json
{
  "receiver_name": "Jane Smith",
  "receiver_acc_no": "0987654321",
  "amount": 50000,
  "method": "UPI"
}
```

---

## Error Response

```json
{
  "timestamp": "2026-07-14T10:15:30",
  "status": 400,
  "error": "Bad Request",
  "message": "Sender account number must contain exactly 10 digits",
  "path": "uri=/api/transfer"
}
```

---

# 🧪 Example cURL Request

```bash
curl -X POST http://localhost:8082/api/transfer \
-H "Content-Type: application/json" \
-d '{
  "sender_name":"John Doe",
  "acc_no":"1234567890",
  "bank_name":"HDFC Bank",
  "amount":150000,
  "receiver":{
      "receiver_name":"Jane Smith",
      "acc_no":"0987654321",
      "bank_name":"ICICI Bank"
  }
}'
```

---

# ✅ Validation Rules

| Field | Validation |
|-------|------------|
| Sender Name | Alphabets and spaces only |
| Receiver Name | Alphabets and spaces only |
| Sender Account Number | Exactly 10 digits |
| Receiver Account Number | Exactly 10 digits |
| Sender Bank Name | Cannot be empty |
| Receiver Bank Name | Cannot be empty |
| Amount | Must be greater than 0 |
| Sender & Receiver Account | Must not be the same |

---

# 💻 Interactive Console

When the application starts, a terminal-based menu is displayed.

```text
================================
 IndiaPaymentRouter - Main Menu
================================
1. New Transfer
2. View Transfer History
3. Exit
--------------------------------
```

### 1. New Transfer

- Enter sender details
- Enter receiver details
- Enter transfer amount
- Application validates inputs
- Appropriate payment method is selected automatically

### 2. View Transfer History

Displays all successful transfers made during the current console session.

### 3. Exit

Closes the console while keeping the REST API running on port **8082**.

---

# ⚙️ Configuration

`src/main/resources/application.properties`

```properties
spring.application.name=india-payment-router
server.port=8082
```

---

# 📌 Business Rules

- Amount must be greater than zero.
- Sender and receiver accounts cannot be identical.
- Invalid sender details are rejected.
- Invalid receiver details are rejected.
- Payment method is selected automatically based on the transfer amount.

---

# 📖 Payment Method Selection

| Amount | Selected Method |
|---------|-----------------|
| ₹50,000 | UPI |
| ₹90,000 | UPI |
| ₹1,50,000 | IMPS |
| ₹2,00,000 | IMPS |
| ₹3,25,000 | NEFT |

---

# 👨‍💻 Author

**Dhiren D**

B.E – CSE
Saveetha Engineering College

---

## ⭐ If you found this project useful, consider giving it a Star on GitHub!
