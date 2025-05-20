
# **How to Disable Right-Click, Inspect Element, and Developer Tools in JavaScript**

## **Introduction**

Many website owners want to  **protect their content**  from being copied, inspected, or downloaded. While  **JavaScript can restrict right-click and block DevTools shortcuts**, it’s important to understand that  **no method is 100% foolproof**—determined users can bypass these restrictions.

This guide covers:  
✔  **Disabling right-click (context menu)**  
✔  **Blocking F12, Ctrl+Shift+I, and other DevTools shortcuts**  
✔  **Detecting DevTools via window resizing**  
✔  **Preventing  `view-source:`  access**  
✔  **Handling users who disable JavaScript**

----------

## **1. Disabling Right-Click (Context Menu)**

Using JavaScript, we can prevent the default context menu from appearing:

```js
document.addEventListener('contextmenu', (e) => {
  e.preventDefault();
  alert("Right-click is disabled!");
});
```

**How it works:**

-   The  `contextmenu`  event triggers when a user right-clicks.
-   `e.preventDefault()`  stops the menu from appearing.
    

**Limitation:**  Users can still disable JavaScript in browser settings.

----------

## **2. Blocking Developer Tools Shortcuts (F12, Ctrl+Shift+I, etc.)**

We can intercept keyboard shortcuts used to open DevTools:

```js
document.addEventListener('keydown', (e) => {
  if (
    e.key === 'F12' || 
    e.keyCode === 123 || // F12 key code
    (e.ctrlKey && e.shiftKey && e.key === 'I') || // Ctrl+Shift+I
    (e.ctrlKey && e.shiftKey && e.key === 'J') || // Ctrl+Shift+J (Console)
    (e.ctrlKey && e.key === 'U') // Ctrl+U (View Source)
  ) {
    e.preventDefault();
    alert("Developer tools are disabled!");
  }
});
```

**What this blocks:**

-   **F12**  (Opens DevTools)
    
-   **Ctrl+Shift+I**  (Inspect Element)
    
-   **Ctrl+Shift+J**  (Console)
    
-   **Ctrl+U**  (View Page Source)
    

**Limitation:**  Users can still open DevTools from the browser menu.

----------

## **3. Detecting DevTools via Window Resize**

When DevTools opens, the window size changes. We can detect this and take action:

```js
let windowWidth = window.innerWidth;
window.addEventListener('resize', () => {
  if (window.innerWidth !== windowWidth) {
    alert("DevTools detected! Page will reload.");
    window.location.reload(); // Refresh the page
  }
});
```

**How it works:**

-   DevTools (when docked) changes the viewport width.
    
-   If a resize is detected, the page reloads.
    

**Limitation:**  Doesn’t work if DevTools is undocked.

----------

## **4. Preventing  `view-source:`  Access**

Browsers allow users to  **view page source**  via:

-   `Ctrl+U`  (blocked earlier)
    
-   `view-source:https://example.com`
    

### **Solution 1: Obfuscate/Minify Your Code**

-   Use tools like  **JavaScript Obfuscator**  to make the source unreadable.
    
-   Example:  [https://obfuscator.io](https://obfuscator.io/)
    

### **Solution 2: Server-Side Protection (Apache/Nginx)**

Block  `view-source:`  requests using  `.htaccess`  (Apache):

```bash
RewriteEngine On
RewriteCond %{HTTP_USER_AGENT} "view-source" [NC]
RewriteRule .* - [F]
```

**Limitation:**  Users can still inspect network requests.

----------

## **5. Handling Users Who Disable JavaScript**

If a user disables JavaScript, your protections won’t work.

### **Solution: Redirect to a No-JS Warning Page**

```html
<noscript>
  <meta http-equiv="refresh" content="0; url=/no-js.html" />
</noscript>
```

**Alternative:**  Use server-side checks (PHP/Node.js) to detect if JS is disabled.

----------

## **6. Advanced: Detect DevTools Even When Undocked**

Some libraries (like  `devtools-detect`) can help:
```html
<script src="https://cdn.jsdelivr.net/npm/devtools-detect@3.0.1/index.js"></script>
<script> if (window.devtools.open) {
    alert("DevTools detected!");
    window.location.href = "/blocked.html"; // Redirect
  } </script>
```

**Limitation:**  Can still be bypassed by skilled users.

----------

## **Final Thoughts: Is It Worth It?**

✅  **Works for casual users**  (prevents basic copying).  
❌  **Not secure for sensitive content**  (use  **DRM**  for videos).  
⚠  **Advanced users can bypass all client-side restrictions.**

### **Best Approach?**

-   **For basic protection:**  Use JS + obfuscation.
    
-   **For strong security:**  Use  **server-side authentication**  +  **DRM**.
    
**full code implementation**🚀
```html
<script>
	// Disable right-click context menu
	document.addEventListener('contextmenu', (e) => {
		e.preventDefault();
		alert("Right-click is disabled!");
		});

		// Disable F12, Ctrl+Shift+I, Ctrl+Shift+J, Ctrl+U
		document.addEventListener('keydown', (e) => {
		// Disable F12
		if (e.key === 'F12' || e.keyCode === 123) {
		e.preventDefault();
		alert("F12 is disabled!");
		}
		// Disable Ctrl+Shift+I (Chrome, Edge, Firefox)
		if (e.ctrlKey && e.shiftKey && e.key === 'I') {
		e.preventDefault();
		alert("Developer Tools are disabled!");
		}
		// Disable Ctrl+Shift+J (Chrome Console)
		if (e.ctrlKey && e.shiftKey && e.key === 'J') {
		e.preventDefault();
		alert("Console is disabled!");
		}
		// Disable Ctrl+U (View Page Source)
		if (e.ctrlKey && e.key === 'u') {
		e.preventDefault();
		alert("View Source is disabled!");
		}
		});
		// Extra: Prevent opening DevTools by checking window size changes (not 100% reliable)
		let  windowWidth = window.innerWidth;

		window.addEventListener('resize', () => {

		if (window.innerWidth !== windowWidth) {
		alert("DevTools detection: Resizing is blocked!");
		window.location.reload(); // Refresh page if DevTools is detected
		}
	});
</script>
```
