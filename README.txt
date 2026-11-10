QIOZO - User Interface and Management
1) Install Node.js 18+ (nodejs.org)
2) In this folder run:  node server.js
3) User Interface:  http://localhost:3000
   Management:      http://localhost:3000/admin   (default password admin123; change with ADMIN_PASS=yourpass node server.js)
Data is saved in db.json next to server.js. Both sites read and write the same data through /api.

Delivery partner location (for the delivery app, built later):
  POST /api/orders/<orderId>/location   header  x-agent: <AGENT_KEY>   body {"lat":..,"lng":..}
  Set the key with:  AGENT_KEY=yourkey node server.js   (default agent123)
Home ads: Management > Home ads (photo or video upload, up to 25 MB). Platform fee and COD handling fee: Management > Store settings.

KEEPING YOUR DATA ON UPDATES
All data (products, orders, users, uploads) lives in the "data" folder next to server.js (data/db.json, data/uploads, data/backups).
To update the app, replace everything EXCEPT the "data" folder. A backup of db.json is saved automatically every 6 hours (last 30 kept).
You can also keep data elsewhere:  DATA_DIR=/path/to/folder node server.js

Delivery partner site:  http://localhost:3000/agent   (login = mobile number + password set in Management > Employment requests / QIOZO employees; job type must be "Delivery partner" and status "Employee").

SETUP (new in this version)
1) Install Node.js 18+, then in this folder:  npm install      (node_modules is already included)
2) Open the file ".env" and check your keys. NEVER share this file or the zip with anyone.
3) Start:  node server.js
   Store: http://localhost:3000   Management: http://localhost:3000/admin   Delivery partner: http://localhost:3000/agent

LOGIN OTP
 - Customers log in with Phone (SMS by Fast2SMS) or Email (Gmail SMTP).
 - Fast2SMS said: "complete website verification" for the OTP route. Do that in Fast2SMS > Dev API > OTP Message (it needs your live website address, localhost cannot be verified),
   or switch FAST2SMS_ROUTE in .env to dlt (needs DLT sender id and template id) or q (Quick SMS).
 - Until SMS works, customers can log in with Email.
 - To test without sending real OTPs set OTP_DEV=1 in .env; the OTP is printed in the server window.

PAYMENTS (Razorpay and Cashfree, test keys in .env)
 - UPI, net banking and cards open the gateway the customer picks. Pay later opens LazyPay through Razorpay
   (LazyPay must be enabled on your Razorpay account). Cash on delivery places the order directly.
 - The order is created only after the payment is verified on the server.
 - Refunds: cancel the order in Management; the customer is told the refundable amount (COD handling and packaging
   charges are non-refundable). Send the money back from the Razorpay / Cashfree dashboard.
 - For live payments replace the test keys with live keys and set CASHFREE_ENV=production. Use HTTPS.

CHANGES IN THIS VERSION
 - Login is by mobile number (SMS OTP) only. Email and WhatsApp OTP are switched off.
 - Customers set their delivery place with "Use my current location" or by typing an address. The distance from the store
   is worked out automatically. In Management > Store settings, set the exact store latitude and longitude.
 - Starting ads: Management > Starting ads. Choose how many (1 to 5) first, then add them.
 - Brand name is QIOZO everywhere.

SHOP HOURS, SCHEDULING, RETURNS
 - Management > Store settings: opening times (default 7:00 AM vegetables, 8:15 AM everything, closes 10:00 PM, India time).
 - Outside hours customers schedule delivery for up to 5 days ahead (same day needs 1.5 hours). Staff call to confirm scheduled orders.
 - Returns: customer taps "Report faulty item" on a delivered order. In Management > Returns & refunds set Approved, Picked up, Refunded. Only the item price is refunded.
 - Pay later (LazyPay): the order is saved as "Pay later pending"; confirm it with LazyPay before delivery. A real LazyPay checkout needs a LazyPay merchant account.

WHAT CHANGED IN THIS VERSION
 - Accounts: Register (name, phone, email, gender, optional photo, password, OTP check) or Login with phone/email + password.
   Phone and email are linked to one account and share one password. "Forgot password" uses an OTP to set a new one.
 - Pack sizes: Management > Products > "Pack sizes" (one per line: label|price|mrp|stock|cost). Each size shows as its own card in the store.
 - Photos are cropped in Management to fixed frames: product 1:1, category 1:1, banner 5:2, home ad 2:1, starting ad 9:16, notification 16:9, logo 1:1.
 - Delivery partners: new orders appear on /agent for every partner; the first to accept gets the order and must reach the store within
   the minutes set in Store settings. Today / month / year totals are on the partner page and in Management > Delivery stats.
 - Cancel: customers can cancel (with a reason) or change the address only until the order is Packed. Management gets a popup and the assigned partner is alerted.
 - Block: Management > Users > "Block & delete" stops that phone/email from logging in or registering again. "Disable COD" turns off cash on delivery for one user.
 - Community: age restriction is a word filter only. There is NO automatic image/video analysis; staff review posts in Management > Community posts.

LATEST CHANGES (batch 3)
 - Emails (Gmail SMTP): order confirmed (with a live-track link that opens the tracker), delivered, cancelled (apology + refund will be initiated),
   refunded, and refund rejected. Set BASE_URL in .env to your real website address so the link works for customers.
   In Management > Orders set "Refund status" to Refunded or Rejected to send the last two emails.
 - Sequence numbers: banners, home ads, starting ads, footer images and categories have a "Seq" box in every row. A number can be used only once.
 - Support: customers use Customer service (3 preset steps, then chat or a call request). Management > Support chats & calls shows them and
   pops up a large alert with a repeating sound for every new chat message or call request (keep the Management page open).
 - Footer images (Available on website / Android) are uploaded in Management > Footer images, with no limit on how many.
 - Delivery time shown under the address is set in Store settings (minutes for the first distance and extra minutes per step).
