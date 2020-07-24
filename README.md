# Watson Assistant Translations from Cloud Functions

This is a [Cloud Function](https://cloud.ibm.com/docs/openwhisk) designed to be used with Watson Assistant and Language Translator in order to translate a conversation with a user from English to their preferred language without the need for an orchestration layer.

## Demo

![translator-bot](/images/translator-bot.gif)

## Prerequisites

- You should have an instance of Watson Assistant, Language Translator, and Cloud Functions created in your IBM Cloud account
- Install the [IBM Cloud CLI](https://cloud.ibm.com/docs/cli)
- Install [Node.js](https://nodejs.org/en/download/)

## Part I: Import the skill to your Watson Assistant Instance

1. If you haven't already, create an instance each of [Watson Assistant](https://cloud.ibm.com/catalog/services/watson-assistant), [Language Translator](https://cloud.ibm.com/catalog/services/language-translator), and [Cloud Functions](https://cloud.ibm.com/functions?bss_account=6b49aa64e559c601cb30fa9027352573&ims_account=1596909)

2. Download the files in this repository: `git clone https://github.com/modlanglais/wa-translator`

3. Import `skill-WA-Translator.json` to your Watson Assistant instance

4. Create and attach an Assistant to your skill and make note of the API key, assistant ID, and URL

## Part II: Create the Cloud Function

5. Run `npm install` in the project directory to install the dependencies

6. In the terminal, login to your IBM Cloud account using the CLI:

    `ibmcloud login`

    If you have trouble, please see the [documentation](https://cloud.ibm.com/docs/cli?topic=cli-getting-started#step3-configure-idt-env). You will need to use the IBM Cloud CLI to point to the namespace in which you created your Cloud Function

7. Issue the following command to push your *new* zipped Cloud Function to your IBM Cloud Functions service:
    `ibmcloud fn action create my-action polyglotbot.zip --kind nodejs:10`

    You can replace `my-action` with whatever you would like to name your cloud function. `polyglotbot.zip` should reflect the name of the .zip file provided

8. (Optional) If you make changes to the files in `polyglotbot.zip` and need to push the new changes, use the following command to *update* your function

    `ibmcloud fn action update my-action polyglotbot.zip --kind nodejs:10`

## Part III: Retrieve your IBM Cloud Function URL

9. Open your Cloud Functions dashboard here: https://cloud.ibm.com/functions/

10. Select the namespace where you deployed function from the drop-down, then click "Actions" on the left nav bar

    ![cf-screenshot](/images/cf-dash.png)

11. Click my-action or the name that you previously gave your action in step 7

12. Select Endpoints then check "Enable as web action" and click "Save"

13. Copy the URL in the "Web Action" section and save for later

    ![cf-webaction](/images/cf-webaction.png)

## Part IV: Add the Webhook to Your Chatbot

14. Open the skill you imported in Watson Assistant earlier

15. Navigate to **Options -> Webhooks**
    ![wa-webhooks](/images/wa-webhooks.png)

16. Replace this URL with your webhook URL that you copied in step 15

17. **Important**: Make sure you append ".json" to the end of this URL in order for Watson Assistant to process the data correctly

18. That's it! Your user may now select their preferred language and Language Translator will automatically translate your English-language chatbot.

## Debugging

To debug and run locally... un-comment the last line in `index.js` which invokes the main() method. Then run as you would any Node app: `node index.js`
