    # Viipie Realty Tools - Setup Instructions

## Gmail OAuth Setup

### Step 1: Create Google OAuth Credentials
1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. 2. Create a new project or select existing one
   3. 3. Go to "Credentials" → "Create Credentials" → "OAuth 2.0 Client ID"
      4. 4. Choose "Web application"
         5. 5. Add Authorized redirect URIs:
            6.    - `https://viipierealty.netlify.app`
                  -    - `https://viipierealty.netlify.app/`
                       -    - `http://localhost:3000`
                            - 6. Copy the Client ID
                             
                              7. ### Step 2: Update OAuth Client ID in Code
                              8. Replace `YOUR_GOOGLE_CLIENT_ID` in `index.html` with your actual Client ID:
                             
                              9. ```html
                                 data-client_id="YOUR_GOOGLE_CLIENT_ID.apps.googleusercontent.com"
                                 ```

                                 ### Step 3: Setup Backend (Optional but Recommended)
                                 Create a Node.js backend to receive user data:

                                 ```javascript
                                 // server.js - Example Express.js setup
                                 const express = require('express');
                                 const cors = require('cors');
                                 const app = express();

                                 app.use(cors());
                                 app.use(express.json());

                                 // Store user logins
                                 const userLogins = [];
                                 const toolUsage = [];

                                 app.post('/api/user-login', (req, res) => {
                                   const userData = req.body;
                                   userData.loginTime = new Date();
                                   userLogins.push(userData);
                                   console.log('User logged in:', userData);
                                   res.json({ success: true });
                                 });

                                 app.post('/api/tool-usage', (req, res) => {
                                   const usageData = req.body;
                                   toolUsage.push(usageData);
                                   console.log('Tool used:', usageData);
                                   res.json({ success: true });
                                 });

                                 app.get('/api/admin/users', (req, res) => {
                                   res.json(userLogins);
                                 });

                                 app.get('/api/admin/usage', (req, res) => {
                                   res.json(toolUsage);
                                 });

                                 app.listen(3000, () => console.log('Server running on port 3000'));
                                 ```

                                 ### Step 4: Update Backend URL in index.html
                                 Replace the fetch URLs in JavaScript:

                                 ```javascript
                                 // Before
                                 fetch('https://your-backend-url.com/api/user-login', ...)

                                 // After (example)
                                 fetch('https://your-nodejs-app.herokuapp.com/api/user-login', ...)
                                 ```

                                 ## Features Implemented

                                 ✅ **Gmail OAuth Login** - Secure authentication with Google accounts (work emails supported)
                                 ✅ **User Data Collection** - Automatic collection of user email, name, profile picture
                                 ✅ **Tool Usage Tracking** - Track which tools are being used and when
                                 ✅ **Local Storage Backup** - User data backed up in browser localStorage as fallback
                                 ✅ **Smaller Tool Tiles** - Compact 4-column grid that adapts to mobile
                                 ✅ **Clean UI** - Minimal, professional design without unnecessary text
                                 ✅ **Back Button** - Auto-appears when viewing tools, logo returns to home
                                 ✅ **Back Button in Tools** - Add this to each tool HTML for back button:

                                 ## Adding Back Button to Tools

                                 Add this code to the beginning of each tool's `<body>` section:

                                 ```html
                                 <div style="background: #fff; padding: 10px 20px; border-bottom: 1px solid #ddd; display: flex; align-items: center; justify-content: space-between;">
                                     <a href="/" style="display: flex; align-items: center; gap: 8px; text-decoration: none; cursor: pointer;">
                                         <div style="width: 32px; height: 32px; background: linear-gradient(135deg, #c9a84c, #a07830); border-radius: 6px; display: flex; align-items: center; justify-content: center; color: #fff; font-weight: bold; font-size: 16px;">V</div>
                                         <span style="color: #1a1208; font-weight: 600;">Back to Tools</span>
                                     </a>
                                 </div>
                                 ```

                                 ## Admin Dashboard Access

                                 User data is stored in localStorage and can be accessed:

                                 ```javascript
                                 // Get all logins
                                 const userLogins = JSON.parse(localStorage.getItem('viipieUser'));

                                 // Get usage history
                                 const usageHistory = JSON.parse(localStorage.getItem('viipieUsage'));
                                 ```

                                 For production, export user data to your backend for permanent storage.

                                 ## Mobile Responsive Design

                                 ✅ Smaller tiles adapt to mobile screens (160px minimum width)
                                 ✅ 4-column grid on desktop, 2-3 columns on tablet, 2 columns on mobile
                                 ✅ Touch-friendly buttons and spacing

                                 ## Security Notes

                                 - OAuth tokens are validated on Google's servers
                                 - - Never store tokens in client-side localStorage (not implemented)
                                   - - Use HTTPS only (automatic on Netlify)
                                     - - Backend should validate all requests
                                       - - Implement rate limiting on backend endpoints
                                        
                                         - ## Next Steps
                                        
                                         - 1. Set up Google OAuth credentials
                                           2. 2. Create backend for user data storage
                                              3. 3. Update Client ID and Backend URLs
                                                 4. 4. Add back buttons to tool HTML files
                                                    5. 5. Test login functionality
                                                       6. 6. Monitor user analytics
