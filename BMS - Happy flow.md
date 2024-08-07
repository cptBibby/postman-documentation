# Current Happy Path

1. **Mock Payment Using Admin API**:  
   Initiate a mock payment request via the admin API.

2. **Receive Deposit Notification**:  
   A deposit notification will be received on the WebSocket.

3. **Check Created Reward**:  
   Verify that the created reward appears in both `rewards()` and `availableRewards()`.

4. **Use Reward**:  
   Use the reward with the ID obtained from the `availableRewards()` response.

5. **Check Reward Status**:  
   Ensure that the same reward still appears in both `rewards()` and `availableRewards()`.

6. **Play Game**:  
   Play the game using the link from the `useReward()` response and end the session. *(Note: This step cannot be performed through Postman.)*

7. **Receive Session Finished Notification**:  
   A session finished notification will be received on the WebSocket.

8. **Verify Reward Status**:  
   - `availableRewards()` should now show no results.
   - In `rewards()`, the reward should be listed as finished and have prizes won under the `prizes` property.

9. **Check Cashback Prizes**:  
   If any cashback prizes were won during the session, they should be shown in the `prizes()` response.

# How to Generate Tokens

1. **gateway_jwt_token**:
   - **Payload**:
     ```json
     {
       "userID": "[your softswiss user id]",
       "sessionID": "[your session id]"
     }
     ```
   - **Secret**: IRONMAN_JWT_SECRET environment variable

2. **admin_jwt_token**:
   - **Payload**:
     ```json
     {
       "userID": "[your softswiss user id]",
       "sessionID": "[your session id]",
       "email": "[any email address ending in @bitstarz.com]"
     }
     ```
   - **Secret**: IRONMAN_JWT_SECRET environment variable

3. **realtime_jwt_token**:
   - **Payload**:
     ```json
     {
       "userID": "[your softswiss user id]",
       "sessionID": "[your session id]"
     }
     ```
   - **Secret**: IRONMAN_CENTRIFUGO_JWT_SECRET environment variable

# How to Setup a WebSocket Connection in Postman

1. **Create a New WebSocket Request**:
   - Go to `New` -> `WebSocket`.

2. **Enter URL**:
   ```plaintext
   {{realtime_hostname}}/connection/websocket?cf_ws_frame_ping_pong=true

3. **Input messages and subscribe to channels**:
   ```plaintext
    {"id":1,"connect":{"data":{"jwt":"{{realtime_jwt_token}}"}}}
    {"subscribe":{"channel":"public:attributes"},"id":2}
    {"subscribe":{"channel":"reward#{{softswiss_user_id}}"},"id":3}
    {"subscribe":{"channel":"bms#{{softswiss_user_id}}"},"id":4}
    {"subscribe":{"channel":"payment#{{softswiss_user_id}}"},"id":5}
   
4. Establish the Connection by clicking Send (currently it will disconnect automatically after 10 minutes)
   
  
