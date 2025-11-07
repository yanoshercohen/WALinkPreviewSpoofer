# WALinkPreviewSpoofer
Script that attempts to spoof the title and description of link previews in WhatsApp Web messages.
<img width="216" height="119" alt="image" src="https://github.com/user-attachments/assets/ba28743e-ce73-4b76-8976-538b124bb038" />

> [!NOTE] 
> This script modifies the link preview's title and description *before* the message is sent. However, the server or receiving clients might re-fetch the preview based on the URL itself, potentially overriding these client-side changes. Therefore, the spoofed preview might not appear to the recipient. The original link destination remains unchanged.
> [!CAUTION] 
> This JavaScript code is for **research purposes** only and is not intended to be used as a basis for any commercial or non-research purposes.  
> The use of this code is at the user's own risk, and the author(s) assume no responsibility for any misuse or unintended consequences.  
> **By using this code, the user acknowledges and agrees to the following:**
> 
> - The user is solely responsible for any legal consequences arising from the use of this code.  
> - The user will not hold the author(s) liable for any damages, losses, or legal actions resulting from the use of this code.  
> - The user agrees to comply with any applicable laws and regulations, including but not limited to copyright, intellectual property, and privacy laws.  
> - Furthermore, the user acknowledges that using this code may violate WhatsApp's Terms of Service and could potentially lead to account bans or other penalties.  
> - The user is advised to consult their own legal counsel before using this code to ensure compliance with all applicable laws and regulations.  
## Usage
1. Open WhatsApp Web in your browser.
2. Press F12 to open the developer tools.
3. Go to the `console` tab.
4. Paste the script.
```js
(() => {
    const WAWebSendMsgRecordAction = require('WAWebSendMsgRecordAction');
    const originalSendMsgRecord = WAWebSendMsgRecordAction.sendMsgRecord;

    // Spoofed data (fallbacks)
    const SPOOFED_TITLE = "Title Here";
    const SPOOFED_DESCRIPTION = "Description Here";

    WAWebSendMsgRecordAction.sendMsgRecord = async function(msg) {
        // Check for URL in body and existing preview data
        if (msg?.__x_body && /https?:\/\/[^\s/$.?#].[^\s]*/i.test(msg.__x_body) &&
            msg.__x_title?.sentinel !== 'DEFAULT VALUE PLACEHOLDER' &&
            msg.__x_description?.sentinel !== 'DEFAULT VALUE PLACEHOLDER' &&
            msg.__x_matchedText?.sentinel !== 'DEFAULT VALUE PLACEHOLDER') {

            console.log(`%c[PREVIEW_SPOOF]%c Attempting to spoof preview for: ${msg.__x_matchedText}`, 'font-weight: 900; font-size: 16px; color: #00ff88;', '');

            // Modify title and description only
            msg.__x_title = SPOOFED_TITLE;
            msg.__x_description = SPOOFED_DESCRIPTION;

            console.log(`%c[PREVIEW_SPOOF]%c Spoofed title: ${msg.__x_title}`, 'font-weight: 900; font-size: 16px; color: #00ff88;', '');
            console.log(`%c[PREVIEW_SPOOF]%c Spoofed description: ${msg.__x_description}`, 'font-weight: 900; font-size: 16px; color: #00ff88;', '');
        }
        return originalSendMsgRecord.apply(this, arguments);
    };
})();
```
