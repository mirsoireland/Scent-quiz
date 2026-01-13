MÍR SÓ Scent Finder Quiz - README
A beautiful, mobile-first fragrance quiz that recommends MÍR SÓ Woody, Fresh, or Citrus based on personality and preferences. Built with vanilla HTML, CSS, and JavaScript—no external dependencies.
Features
✨ Interactive Quiz - 9 questions with smooth transitions
📊 Smart Scoring - Weighted answers with tie-breaking logic
🎯 Personalised Results - Custom fragrance notes, tone, and why it suits you
🛒 Direct Shopping Links - "View MÍR SÓ [Scent]" buttons drive sales
📧 Email Capture - Optional consent-based email collection
📈 Basic Analytics - Local storage tracks quiz performance
🎨 Brand Customisation - Colour-coded results matching each scent
♿ Accessible - Keyboard-friendly, screen reader compatible
Quick Start
1.  Save the file as scent-quiz.html
2.  Open in browser to test locally (double-click the file)
3.  Customise URLs (see below)
4.  Upload to your web host or use a free service like Netlify Drop
----
Customisation Guide
1.  Fix Product URLs (Important!)
The #1 issue is broken product links. You must update these URLs to match your actual product pages:
Find the  fragranceData  object (around line 308) and replace the three URLs:
const fragranceData = {
woody: {
// ... other data ...
url: "https://mirsoireland.com/products/YOUR-WOODY-PRODUCT-HANDLE"
},
fresh: {
// ... other data ...
url: "https://mirsoireland.com/products/YOUR-FRESH-PRODUCT-HANDLE"
},
citrus: {
// ... other data ...
url: "https://mirsoireland.com/products/YOUR-CITRUS-PRODUCT-HANDLE"
}
}
Don't know your product handles?
Go to each product page on your site and copy the URL from your browser. Examples:
•  If your product page is: https://mirsoireland.com/products/mir-so-30ml-eau-de-parfum
•  And you use variants: You likely need one URL with variant selection
For a single product page with variants, change the url to your main product URL and add variant logic:
// In the shop button click handler (around line 580), change:
window.open(document.getElementById('shopUrl').value, '_blank')
// To something like:
const scent = document.getElementById('shopFragranceName').textContent.toLowerCase();
window.open(https://mirsoireland.com/products/mir-so-30ml-eau-de-parfum?variant=${scent}, '_blank')
(You'll need to check your Shopify/website variant parameter structure)
2.  Replace Bottle Images
Currently uses coloured blocks as placeholders. To add real product photos:
Option A: CSS Background Images
Replace the .bottle-visual CSS classes (around line 50):
.bottle-visual.woody {
background-image: url('images/woody-bottle.jpg');
background-size: cover;
background-position: center;
/* remove the gradient background */
}
Option B: HTML <img> tags
In the HTML section, replace:
With:
<img src="images/woody-bottle.jpg" alt="MÍR SÓ Woody bottle" class="bottle-visual woody">
(Don't forget to upload your images to the same folder or update the path)
3.  Change Colours
Each scent has a colour theme defined in CSS variables (lines 7-10):
--woody-colour: #8b7355;  /* Warm neutral /
--fresh-colour: #a8c8ec;  / Clean light blue /
--citrus-colour: #ffb347; / Bright orange */
Change these hex codes to match your brand colours.
4.  Edit Questions or Scoring
Questions are in the questions array (around line 340). Each question looks like:
{
id: 1,
text: "Your vibe on a normal day",
answers: [
{ text: "Clean and bright", points: { fresh: 3, woody: 0, citrus: 0 } },
{ text: "Warm and grounded", points: { fresh: 0, woody: 3, citrus: 0 } },
{ text: "Zesty and lively", points: { fresh: 0, woody: 0, citrus: 3 } }
]
}
•  Edit text: Change the text fields
•  Change scoring: Adjust the points values (higher numbers = stronger preference)
Email Capture Setup
The quiz collects emails in localStorage by default (browser-only). To make it functional:
For Klaviyo:
Replace the submitEmail() function (around line 620) with:
function submitEmail() {
const email = document.getElementById('emailInput').value;
const consent = document.getElementById('consentCheck').checked;
if (!email || !email.includes('@')) {
    alert('Please enter a valid email address');
    return;
}

if (!consent) {
    alert('Please consent to receiving updates');
    return;
}

// Klaviyo API call
fetch('https://a.klaviyo.com/api/v2/list/LIST-ID/subscribe', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
    },
    body: JSON.stringify({
        api_key: "YOUR-KLAVIYO-PRIVATE-API-KEY",
        profiles: [{
            email: email,
            properties: {
                scent_result: document.getElementById('primaryFragrance').textContent,
                quiz_completed: new Date().toISOString()
            }
        }]
    })
}).then(response => {
    if (response.ok) {
        document.getElementById('emailSuccess').style.display = 'block';
        document.getElementById('emailInput').value = '';
        document.getElementById('consentCheck').checked = false;
    }
});

}
(Replace LIST-ID and YOUR-KLAVIYO-PRIVATE-API-KEY with your actual values)
Analytics
The quiz automatically tracks:
•  Total quizzes taken
•  Which scent wins most often
•  Which questions influence decisions most
View analytics: Press the faint "Debug" button in the bottom-right corner when viewing results.
To collect data: The analytics store in the user's browser. For business insights, send this data to your backend by adding a fetch() call in the showResults() function.
Troubleshooting
Problem	Solution
404 Error on product link	Update the url in fragranceData to match your exact product page URLs
Quiz won't start	Check your browser's JavaScript console (F12) for errors
Images not showing	Verify image paths are correct and files are uploaded
Styling looks wrong	Ensure all CSS is between <style> tags and saved in the same file
Results seem off	Check the points values in the questions array for balance
Going Live
Option 1: Upload to your web host
•  Upload scent-quiz.html via FTP/cPanel
•  Access at https://yourdomain.com/scent-quiz.html
Option 2: Netlify Drop (Easiest)
1.  Go to app.netlify.com/drop
2.  Drag and drop the HTML file
3.  Get a live URL instantly
4.  Share on social media
Option 3: Shopify Page
5.  In Shopify admin, go to Online Store > Pages > Add Page
6.  Click the <> icon to edit HTML
7.  Paste the entire code
8.  Save and add to your navigation
----
Support
If you encounter issues:
1.  Check the Troubleshooting table above
2.  Verify all URLs are correct
3.  Test in an incognito browser window
4.  Check the JavaScript console (F12) for errors
For technical help, share the error message from the console when asking for assistance.
