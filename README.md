 # 🚚 TranxortRider Admin Panel Documentation

## 📖 Overview

This documentation outlines the database structure, admin panel functionality, and technical requirements for the **TranxortRider Admin Panel**. The panel manages delivery riders, verifies documents, assigns orders, monitors deliveries in real-time, and handles earnings through integration with Firebase and Supabase.

---

## 🗂️ Database Structure

TranxortRider uses **Firebase Firestore** for real-time database needs and **Supabase Storage** for managing uploaded documents.

### 🔹 Collections Summary

- **`users`**: Rider account details
- **`riders`**: Rider-specific application & status details
- **`orders`**: Delivery orders and assignment info
- **`userDocuments`**: Uploaded verification documents
- **`admins`**: Admin user accounts and permissions
- **`batches`**: Grouped orders for efficient delivery
- **`earnings`**: Rider earnings logs
- **`rider_locations`**: Rider location tracking
- **`admin_logs`**: Admin activity logs
- **`notifications`**: Notification messages for users

Refer to the [full database schema](#database-schema-details) below for field-level information.

---

## 🔧 Admin Panel Functions

### 1. 🧑‍💼 Rider Management

- **View & Review Applications**
- **Approve/Reject Documents**
- **Track Rider Status & Location**
- **Suspend/Reactivate Riders**

### 2. 📦 Order Management

- **View & Assign Orders**
- **Track Order Status**
- **Unassign & Reassign Deliveries**
- **Generate Order Reports**

### 3. 💰 Earnings Management

- **Track Rider Earnings**
- **Generate Payment Reports**
- **Handle Payment Disputes**

### 4. 📊 Analytics & Reports

- **Delivery Metrics & KPIs**
- **Earnings Reports**
- **Rider Performance Monitoring**

### 5. 🛡️ Admin User Management

- **Create/Edit Admins**
- **Assign Permissions**
- **Log Admin Activity**

---

## 🔐 Security Rules (Firestore)

```js
// Admin Access Check
function isAdmin() {
  return isAuthenticated() &&
    exists(/databases/$(database)/documents/admins/$(request.auth.uid));
}

// Permission Check
function hasPermission(permission) {
  return isAdmin() &&
    get(/databases/$(database)/documents/admins/$(request.auth.uid))
      .data.permissions[permission] == true;
}
```

Rules enforce permission-based access for:
- Rider document approvals
- Order assignments
- Admin activity logs

---

## 🗃️ Supabase Document Storage

- **Storage Bucket**: `riders-info`
- **Path**: `riders/{userId}/documents/{fileName}`
- **Naming Format**: `{documentType}_{timestamp}_{uuid}.{ext}`

```kotlin
// Accessing Supabase document
val publicUrl = documentsBucket.publicUrl(storagePath)
documentsBucket.delete(storagePath)
```

---

## 🔴 Real-Time Updates

- **Pending Rider Applications Listener**
- **Live Rider Location Tracking**
- **Automatic Dashboard Refreshes**

```kotlin
db.collection("riders")
  .whereEqualTo("applicationStatus", "pending")
  .addSnapshotListener { ... }

db.collection("admin_rider_tracking")
  .whereGreaterThan("timestamp", System.currentTimeMillis() - 30 * 60 * 1000)
  .addSnapshotListener { ... }
```

---

## 🔔 Admin Notifications

Admins receive real-time alerts on:

- New riders ready for review
- Rejected document feedback
- Order assignment changes

```kotlin
val notification = mapOf(
  "type" to "rider_ready_for_review",
  "title" to "Rider Ready for Review",
  ...
)
```

---

## 🔌 Technical Stack

- **Frontend**: React / Kotlin (optional depending on implementation)
- **Backend**:
  - Firebase Authentication
  - Firebase Firestore (DB)
  - Supabase Storage
  - Cloud Functions (for automation & processing)
- **Real-Time Updates**: Firestore listeners

---

## 📘 Database Schema Details

Refer to [full schema](#) for detailed field structures of each collection:
- [users](#)
- [riders](#)
- [orders](#)
- [userDocuments](#)
- [admins](#)
- [batches](#)
- [earnings](#)
- [rider_locations](#)
- [admin_logs](#)
- [notifications](#)

---

## ✅ Conclusion

This documentation provides all the structural and functional insights required to develop the **TranxortRider Admin Panel**. By integrating Firebase and Supabase effectively, the panel will offer scalable, secure, and real-time capabilities for managing the rider delivery ecosystem.

---
