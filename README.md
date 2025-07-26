# kiro-auto-run

A tiny console snippet that automatically clicks the **Run** button in Kiro IDE, so you can focus on coding without manual “Run” clicks.

---

## Description

When working in Kiro IDE, every code change often requires clicking the **Run** button (`button.kiro-button`). This script watches the page and auto‑clicks “Run” as soon as it appears, eliminating repetitive clicks.

## ⚠️ Important: Script Persistence

Scripts pasted into the Developer Console are **not** saved across reloads. To run automatically on startup, use a user‑script manager like **Tampermonkey** or **Greasemonkey**.

### For Permanent Auto‑Loading (Optional)

1. Install **Tampermonkey** (or your preferred manager).
2. Create a new user script and copy the contents of [kiro-auto-run.js](kiro-auto-run.js).
3. Save and enable the script; it will inject automatically each time you open Kiro IDE.

---

## Installation

### Console Paste Method (Temporary)

1. Open **Kiro IDE** (web or desktop).
2. In **Kiro IDE**, go to the **Help** menu and select **Toggle Developer Tools**.
3. Switch to the **Console** tab.
4. Paste in the snippet below and press **Enter**:

```js
(function(){
  const COOLDOWN_MS = 2500;
  let lastClick = 0;

  function clickRunIfFound() {
    const now = Date.now();
    if (now - lastClick < COOLDOWN_MS) return;

    const runBtn = Array.from(
      document.querySelectorAll('button.kiro-button')
    ).find(el => /^Run$/i.test(el.textContent?.trim()));

    if (runBtn) {
      runBtn.click();
      lastClick = now;
      console.log('[auto] Clicked Run');
    }
  }

  const intervalId = setInterval(clickRunIfFound, 1000);
  const observer   = new MutationObserver(clickRunIfFound);
  observer.observe(document.body, { childList: true, subtree: true });

  window.stopAutoRun = () => {
    clearInterval(intervalId);
    observer.disconnect();
    console.log('[auto] stopped.');
  };

  console.log('[auto] Kiro Run‑button autoclicker started.');
})();
```

> **Based on**: Auto‑Clicker Bookmarklet structure ([github.com](https://github.com/sparemind/AutoClickerBookmarklet?utm_source=chatgpt.com))

---

## Usage

* Script polls every second (and on DOM changes) for any `<button class="kiro-button">` whose text is exactly “Run”.
* It clicks the button (respecting a 2.5 s cooldown) and logs each click.

## Stopping

To halt the auto‑clicker, run:

```js
window.stopAutoRun();
```

---

## Contributing

Pull requests and issues are welcome! Feel free to:

* Adjust the cooldown interval
* Support other button variants (e.g., different sizes or variants)
* Provide installation via other loaders

---

## License

MIT © Your Name
