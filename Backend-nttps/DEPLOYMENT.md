# Authentication deployment checklist

The portal login page uses the Render service at `https://nttps-backend.onrender.com` for registration and password-reset OTPs. The browser will return **404 Not Found** until the backend containing these routes is deployed.

## Deploy the backend

1. Deploy the current branch of this repository to the Render service that serves `nttps-backend.onrender.com`.
2. Set the environment variables from `.env.example`. In particular, set `JWT_SECRET` to a random value of at least 32 characters and set `REGISTRATION_INVITE_CODE`.
3. Keep the existing `EMAIL_USER` and `EMAIL_PASS` Gmail app-password configuration. The backend sends both registration and password-reset OTP emails using this SMTP account.
4. Redeploy or restart the service after changing environment variables.

## Verify after deployment

Use a real registered email address when testing. Do not use a production user's email for automated tests.

```bash
curl -i -X POST https://nttps-backend.onrender.com/password-reset-send-otp \
  -H 'Content-Type: application/json' \
  --data '{"email":"registered-user@example.com"}'
```

The endpoint should respond with HTTP `200` and a generic confirmation message. A `404` means Render is still serving an old backend deployment, not that the frontend needs to use the separate `nttps-email` repository.
