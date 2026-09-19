# WORKLOG - MSAII LLC one page site

Each line: what I did -> the command I ran -> what it actually printed.

- Listed the pictures folder -> `Get-ChildItem -LiteralPath ".\Images"` -> 13 jpeg files; member afterwards renamed two to `Logo_BlackBG.jpeg` and `Logo_whiteBG.jpeg` (12 rows returned on the second listing).
- Read the small pictures to identify them -> `read image_20260919111036.jpeg`, `read image_20260919111058.jpeg`, `read photo kumar.jpeg` -> dark-background logo, light-background logo, and a real portrait.
- Read the remaining pictures to place them and write alt text -> `read Founder_presenting_numbers_to_cl...jpeg`, `read Founder_portrait...jpeg`, `read Founder_working_at_desk...jpeg`, `read Laptop_mockup_with_phone_dashboard...jpeg`, `read Before_and_after_dashboard_compa...jpeg`, `read Alert_card_on_glass_panel...jpeg`, `read Business_owner_standing_in_office...jpeg`, `read Floating_interface_cards_flat_lay...jpeg`, `read People_viewing_laptop_dashboard...jpeg`, `read Person_working_at_office_desk...jpeg` -> all on-brand MSAII dashboards, offices and the founder.
- Checked tooling -> `gh auth status`, `git --version`, `chrome` at `C:\Program Files\Google\Chrome\Application\chrome.exe` -> logged in as keekumar15-cpu, git 2.55.0, Chrome present.
- Copied and renamed pictures into the site -> `Copy-Item` map `Logo_whiteBG.* -> logo.jpeg` etc. -> 11 files; dimensions `logo 1024x1024`, `speaking 2752x1536`, `working 1792x2400`, other shots `2048x2048`.
- Cropped the logo to its content so the mark reads at 40px -> System.Drawing content-box scan -> content box `x:102-920 y:392-632`, wrote `logo.jpeg` and `logo-dark.jpeg` at `268x268` and `272x272`.
- Served the page and fetched it -> `node static-server.mjs . 4173`; `Invoke-WebRequest http://localhost:4173/` -> `HTTP 200 bytes=20080`, title `MSAII LLC - One clear monthly numbers dashboard`.
- Screenshotted the page -> `chrome --headless=new --window-size=1440,900 --screenshot=...` -> `767167 bytes written`, then re-shot after fixes with a cache-busting `?v=` query.
- Measured the page at phone width -> CDP `Emulation.setDeviceMetricsOverride 390` -> `{"innerWidth":390,"scrollWidth":390,"offenders":[]}`.
- Tested a missing picture -> renamed `working.jpeg` aside, loaded, evaluated -> `hiddenImages:["images/working.jpeg"]`, `aboutHeading:"Why I started MSAII LLC"`, `aboutParagraphs:2`, `scrollWidth:1425`; then restored the file.
- Checked every image URL -> `Invoke-WebRequest -Method Head` for each `src` -> all 11 returned `HTTP 200 image/jpeg`.
