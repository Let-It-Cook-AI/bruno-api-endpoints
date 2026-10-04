# Let It Cook API (Bruno)

Requests for every core-api endpoint, in Bruno's OpenCollection (YAML) format.

## Setup

1. Open `Let It Cook/` as a workspace in Bruno and pick the `local` or `production` environment.
2. In the environment, fill in the secret variables. Bruno keeps their values on your machine,
   never in these files:
   - `firebaseApiKey`: the Firebase Web API key (Firebase console, Project settings)
   - `email` and `password`: a test account
3. Run **Auth / Sign In**. It stores a Firebase ID token in `idToken`, which every other request
   sends. Tokens expire after an hour; run it again when requests start answering 401.
4. Set `recipeId` to one of your recipes for the requests that need one.

Register, Reset Password and Confirm Password Reset are public and send no token.

## Notes

- The streaming requests (`Stream …`) answer with Server-Sent Events; Bruno shows the raw event
  stream once it ends. Each sends a fresh `requestId`, as the app does.
- AI requests count toward the account's daily limit (50 by default).
