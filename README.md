# LivePad

A single-file, password-gated, live collaborative notepad with an Obsidian dark theme. Static site, works on GitHub Pages.

## How it works

- No backend and no accounts. Sync is peer-to-peer over WebRTC via the free public PeerJS broker.
- The password (plus optional note name) derives the room ID and an AES-GCM key (PBKDF2, 120k iterations). All note traffic is end-to-end encrypted; the broker and anyone without the password see only ciphertext.
- First person online hosts the room. Others with the password join it. Both sides edit live.
- If the host leaves, another open client automatically takes over hosting. Each browser also keeps an encrypted copy in localStorage.
- Wrong password: decryption fails and the user is bounced back to the lock screen.

## Deploy to GitHub Pages

```bash
git init
git add index.html README.md
git commit -m livepad
git branch -M main
git remote add origin git@github.com:<you>/livepad.git
git push -u origin main
```

Then: repo Settings > Pages > Source: `main` branch, `/ (root)`. Your pad is live at `https://<you>.github.io/livepad/`.

Or with the GitHub CLI:

```bash
gh repo create livepad --public --source=. --push
```

Then enable Pages as above.

## Usage

1. Open the page, pick a note name (optional) and a password.
2. Share the URL, note name, and password with your collaborator.
3. Type. Both sides see edits within a fraction of a second.

## Notes and limits

- All sync is P2P: at least one participant must have the page open for the room to be reachable. If you open it alone later, your last locally saved copy is there and you become the host.
- Not end-to-end audited; use a strong password. Anyone with the password can read and edit.
- One room hosts one note. Use different note names for multiple pads.
