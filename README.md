# REC CampusConnect

A college portal built with Google Apps Script and Google Sign-In.

## Features

- College logo is static (no rotation animation).
- Unnecessary circular loader below the logo is removed.
- Google Sign-In is used for student and administrator authentication.
- Student accounts must use a verified Google account ending in `@rajalakshmi.edu.in`.
- Admin access is granted only to exact addresses configured in the `ADMIN_EMAILS` Apps Script property.
- Contact Admin opens an email addressed to `admin@rajalakshmi.edu.in`.

## Project files

- `Code.gs` — Apps Script backend.
- `index.html` — portal frontend.
- `appsscript.json` — Apps Script project configuration.

## Setup

1. Open the Google Apps Script project associated with the portal.
2. Add or replace `Code.gs`, `index.html`, and `appsscript.json` with the files in this project.
3. In **Project Settings → Script Properties**, add:
   - `GOOGLE_CLIENT_ID`: your Google OAuth Web client ID.
   - `ADMIN_EMAILS`: `admin@rajalakshmi.edu.in` (use comma-separated exact addresses only if more admins are authorized).
4. Configure the OAuth consent screen and Web application OAuth client in Google Cloud Console. Ensure the client ID matches `GOOGLE_CLIENT_ID`.
5. Run `setupPortal()` once from Apps Script and approve the requested permissions.
6. Deploy as a Web app. For the Sheets-backed setup, use **Execute as: Me** and select the narrowest access audience compatible with your institution's sign-in requirements.
7. Deploy a **new version** and test with a real student Google account and the configured admin account.

## Authentication notes

- The admin account signs in with Google; the previously supplied password is not used or stored in this Google-only authentication build.
- Do not put OAuth secrets or passwords in `index.html`, source control, or this README.
- Do not trust a role sent by the browser. Admin authorization must be checked by the backend against `ADMIN_EMAILS`.
- Google token verification through `tokeninfo` is suitable for a prototype, not a high-assurance production identity service. For production, use a supported server-side token verification library/service and institutional identity controls.
- Back up existing Sheets data before migrating. Legacy password-based accounts may not work with Google-only authentication.
- Never share the Apps Script editor or backing spreadsheet with students.

## Student profile data

Google Sign-In provides verified identity information such as name and email. It does not necessarily provide register number, department, year, or phone. Populate those fields from a protected institutional roster or collect them separately and verify them.

## Contact Admin

The Contact Admin action is configured for `admin@rajalakshmi.edu.in`. Test it on a device with a default mail application configured. If the portal is hosted in a restricted browser environment, verify that `mailto:` links are permitted.

## Before production launch

- Verify OAuth client ID and authorized origins.
- Verify student-domain and admin-allowlist enforcement on the server.
- Test unauthorized, unverified, expired-token, and non-college accounts.
- Review access permissions for Apps Script and Google Sheets.
- Test the deployed URL on desktop and mobile.
