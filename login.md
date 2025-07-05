# Login

The **Login** feature allows registered users to securely access their accounts and start engaging with support services or managing chats.

---

## Where to Access

- **URL:** [https://support.microdeets.com](https://support.microdeets.com)
- **Method:** Web-based access through any modern browser

---

## Login Page Overview

The login interface is designed with user experience and security in mind.

| Field        | Input Type | Required | Description                                 |
|--------------|------------|----------|---------------------------------------------|
| **Email**    | Text       | ✅        | Your registered email address               |
| **Password** | Password   | ✅        | Your secure account password (hidden input) |

📸 **Sample View**:  
![Login Page Screenshot](assets/login.png)

---

## How to Log In

Follow these easy steps:

1. **Go to** [https://support.microdeets.com](https://support.microdeets.com)
2. Enter your **Email Address**
3. Enter your **Password**
4. Click the **Login** button
5. If authenticated, you'll be redirected to the **Dashboard**
6. If the login fails, a relevant error message will guide you

---

## Error Handling

| Scenario            | Message Displayed             | Action Required                        |
|---------------------|-------------------------------|----------------------------------------|
| Invalid credentials | "Invalid email or password"   | Re-enter correct credentials           |
| Inactive account    | "Account is inactive"         | Contact support team for activation    |
| Missing fields      | Submission is blocked         | Fill in all required fields            |
| Server issues       | "Server error"                | Try again later or contact support     |

---

## Security & Best Practices

- 🔐 **JWT Authentication:** Secure token issued after successful login
- 👀 **Hidden Password Field:** Prevents password exposure
- ⏱️ **Token Expiry:** Sessions expire after 1 hour of inactivity
- ❌ **Hashed Passwords:** Stored securely with bcrypt
- 🔐 **Account Restrictions:** Inactive accounts cannot log in

---

## Tips for Users

- Use a **strong password** with a mix of uppercase, lowercase, numbers, and symbols
- Only enable browser autofill on **trusted devices**
- Use the **"Forgot Password"** feature if needed
- Contact support if your account status is **inactive**

---

## Backend Technical Overview

- **Platform:** Node.js (Express)
- **Authentication:** Handled by `/api/auth/login` endpoint using JWT:contentReference[oaicite:0]{index=0}
- **Database:** MySQL with Sequelize ORM:contentReference[oaicite:1]{index=1}:contentReference[oaicite:2]{index=2}
- **Security:** Passwords hashed using bcrypt, authorization via token middleware:contentReference[oaicite:3]{index=3}

---

## 📞 Need Help?

If you're facing any issues with login, contact our support team:

- 📧 **support@microdeets.com**
- 📞 **+880-1611101183**

---
