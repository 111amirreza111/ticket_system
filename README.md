Creating a simple customer support ticket system that sends user submissions to your Gmail account is quite achievable using HTML and a backend solution like Google Apps Script to send the email. Here's how you can set it up for free:

### Steps:

1. Create a simple HTML form (frontend)

   This form will allow the user to input their message and submit it.

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Customer Support Ticket</title>
</head>
<body>
    <h1>Customer Support Ticket</h1>
    <form id="supportForm">
        <label for="name">Your Name:</label><br>
        <input type="text" id="name" name="name" required><br><br>
        
        <label for="email">Your Email:</label><br>
        <input type="email" id="email" name="email" required><br><br>
        
        <label for="message">Your Message:</label><br>
        <textarea id="message" name="message" rows="4" required></textarea><br><br>
        
        <button type="submit">Submit Ticket</button>
    </form>

    <script>
        document.getElementById("supportForm").addEventListener("submit", function(event){
            event.preventDefault(); // Prevent form submission

            const name = document.getElementById("name").value;
            const email = document.getElementById("email").value;
            const message = document.getElementById("message").value;

            fetch("https://script.google.com/macros/s/your-google-script-id/exec", {
                method: "POST",
                body: JSON.stringify({name, email, message}),
                headers: {
                    "Content-Type": "application/json"
                }
            })
            .then(response => response.json())
            .then(data => {
                alert("Ticket submitted successfully!");
            })
            .catch(error => {
                alert("There was an error submitting your ticket.");
            });
        });
    </script>
</body>
</html>
2. Set up Google Apps Script (backend)

   You will use Google Apps Script to process the data and send it to your Gmail.

   a. Open [Google Apps Script](https://script.google.com) and create a new project.
   
   b. Replace the default Code.gs content with the following:

function doPost(e) {
  var data = JSON.parse(e.postData.contents);
  var name = data.name;
  var email = data.email;
  var message = data.message;

  var recipient = "your-email@gmail.com"; // Your Gmail address
  var subject = "New Customer Support Ticket";
  var body = "Name: " + name + "\n" +
             "Email: " + email + "\n" +
             "Message: " + message;

  MailApp.sendEmail(recipient, subject, body);

  return ContentService.createTextOutput(
    JSON.stringify({result: "success"})
  ).setMimeType(ContentService.MimeType.JSON);
}
   c. Save the script and click on Deploy > Test deployments to deploy it as a web app.

   d. Set the "Who has access" to Anyone and "Execute as" to Me (your Gmail account).

   e. Click Deploy and copy the URL provided.

3. Connect HTML form to Google Apps Script

   Go back to your HTML form code and replace your-google-script-id in the fetch URL with the URL from the Google Apps Script deployment step.

4. Test the system

   Now when a user submits the form, their ticket will be sent directly to your Gmail inbox.

### Free Plan Limitations:
- Google Apps Script has a daily email limit. For free accounts, it's 100 emails per day, which is typically enough for a basic ticket system.
  
By using Google Apps Script, this entire system remains free and easy to set up!