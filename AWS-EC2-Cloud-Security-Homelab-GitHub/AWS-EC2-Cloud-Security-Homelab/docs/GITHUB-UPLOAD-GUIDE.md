# Upload this homelab to GitHub

## Upload using the GitHub website

1. Sign in to GitHub and select **New repository**.
2. Use repository name **`AWS-EC2-Cloud-Security-Homelab`**.
3. Set the visibility to **Public** if you want to show it to recruiters.
4. Create the repository without another auto-generated README (this ZIP already has one).
5. **Extract this ZIP** on your computer first.
6. In the new repository, select **Add file → Upload files**.
7. Upload the **contents inside** the extracted `AWS-EC2-Cloud-Security-Homelab` folder, including the `docs`, `website`, and `screenshots` directories. Do not upload just the ZIP unless you only want a downloadable archive.
8. Commit with the message: `Add AWS EC2 cloud security homelab documentation and screenshots`.
9. Open the repository homepage; the images should appear automatically because the README uses relative links.

## Important

Do **not** upload any AWS `.pem` key file, `.aws/credentials`, `.env` containing secrets, or the original full CloudTrail JSON. Double-check screenshots and source files before making the repository public.

This ZIP already includes sanitized screenshots. If you add new screenshots later, redact personal IPs, account IDs, tokens and access identifiers.
