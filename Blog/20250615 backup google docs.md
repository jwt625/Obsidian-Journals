
2025-06-15T20:34:54-07:00

`pip install --upgrade google-api-python-client google-auth google-auth-oauthlib`


2025-06-15T20:42:33-07:00
what kind of bullshit is this
![[Pasted image 20250615204239.png]]


2025-06-15T20:52:28-07:00
ok this is helpful:
- https://takeout.google.com/
![[Pasted image 20250615205234.png]]


But this is only for personal drive. I do want some from the shared drive.
Do I have to get a mouse clicker thingy?

2025-06-15T20:56:42-07:00
Fuck APIs:
⏺ Best automation approaches:

  1. Selenium (Recommended)
  - Most reliable for web automation
  - Can handle authentication
  - Works with dynamic content

  2. Playwright
  - Modern alternative to Selenium
  - Better performance
  - Built-in waiting mechanisms

  3. Mouse/Keyboard Automation
  - PyAutoGUI (fragile, screen-dependent)
  - AppleScript on macOS (limited)

2025-06-15T21:12:19-07:00
```

  pip install playwright
  playwright install chromium
  python playwright_drive.py

```

2025-06-15T22:18:47-07:00
Keep getting
![[Pasted image 20250615221850.png]]
damn google.

This is a common issue with organization-managed Google
  accounts. Google Drive can detect automation in several ways:

  Why "Not Signed In" Appears

  1. Organization Security Policies
  - Your org likely has policies blocking automated access
  - Even with manual login, Drive detects the automated browser
  - Managed accounts have stricter security than personal ones

  2. Automation Detection
  - Google detects Playwright/automation markers
  - Browser fingerprinting identifies non-human usage
  - Session validation fails automated checks

  3. Cookie/Session Issues
  - Automation browsers often have isolated cookie stores
  - Drive requires specific session tokens
  - Cross-domain authentication may fail

  Solutions to Try

  Option 1: Use Your Personal Profile
  # Modify the browser setup to use your existing Chrome profile
  async def setup_browser(self, headless=False):
      playwright = await async_playwright().start()

      # Use your existing Chrome profile
      self.context = await
  playwright.chromium.launch_persistent_context(
          user_data_dir="/Users/wentaojiang/Library/Application 
  Support/Google/Chrome/Default",
          headless=headless,
          args=[
              '--disable-blink-features=AutomationControlled',
              '--disable-web-security'
          ]
      )
      self.page = await self.context.new_page()

  Option 2: Enhanced Stealth Mode
  Want me to update the script with better stealth features?

  Option 3: Alternative Approaches
  - Browser Extension: Create a Chrome extension for downloading
  - Bookmarklet: JavaScript snippet for manual triggering
  - Screen Recording: Capture manual downloads for automation

  Option 4: Organization Workarounds
  - Ask IT to whitelist automation for your account
  - Use a personal Google account to access shared files
  - Request IT to perform the export for you

  Which approach would you like to try first? The personal
  Chrome profile method often works best for managed accounts.


2025-06-15T22:32:30-07:00
Yeees using firebox works.

2025-06-15T22:55:54-07:00
Download working, hell yeah. Slow though, 5~10 s per file.
- 6 months is still 1 million files, guess I should get this start running

2025-06-15T23:58:45-07:00
Going into folders and downloading works. Going back not working.
Stupid piece of shit, why not just use the browser's going back button??

2025-06-16T23:15:33-07:00
Hell yeah fixed the navigation and slow download. It is waiting for a 30 s timeout.

Now debugging the download too fast and start skipping files...
ok fuck this:
![[Pasted image 20250616233110.png]]

2025-06-16T23:37:59-07:00
Hell yeah
```

📥 Downloading file 4/6: 20200122 - LN POM nat comm clean.zip
📄 Downloading file: 20200122 - LN POM nat comm clean.zip
🔄 Files in download folder before: 8
🔄 Expected download location: ./firefox_downloads/20200122 - LN POM nat comm clean.zip
🔄 Right-clicking on file element...
🔄 Found download option with: [aria-label*="Download"]
✅ Clicked download from context menu
✅ Initiated download: 20200122 - LN POM nat comm clean.zip
🔄 Checking for virus scan popup...
🦠 Found virus warning dialog: Can’t scan file for viruses
"20200122 - LN POM nat comm clean.zip" (35.5MB) exceeds the maximum file...
🦠 Found 'Download anyway' button with: text="Download anyway"
✅ Successfully clicked 'Download anyway' button
🔄 Waiting for download completion of: 20200122 - LN POM nat comm clean.zip
🔄 Monitoring folder: ./firefox_downloads
💾 Downloaded: 20200122 - LN POM nat comm clean.zip → /20200122 - LN POM nat comm clean.zip
✅ Download completed: File count increased from 8 to 9
✅ New file detected: 232183_0_attach_6_17344.pdf
⏱️  Rate limit delay: 1.4s (downloaded: 3)
✅ Successfully downloaded: 20200122 - LN POM nat comm clean.zip
```


2025-06-16T23:51:22-07:00
Had to fix it again for all popups.
Seems to run fine now, running on QAOM drive.

