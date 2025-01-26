**Setup Environment** # Create a directory
mk folder_name #make directory
cd folder_name #change directory

    # Create the environment
        python -m venv venv

    # Activate the environment
        .\\venv\\scripts\\activate

    # Create requirements.txt
        flask
        python-dotenv
        requests

    #Install requirements
        pip install -r requirements.txt

**Frontend Development Steps**
Create a app.py and import required libraries from flask
Create a default html and a route in app.py @app.route("/")
Define a function with friendly name and render template so that if the action of a form has "/" or etc it will get the data from there use in the function

**Backend Development Steps**
Create Azure Translator project and follow the steps(which has already trained models)
In the end you will have KEY, ENDPOINT and LOCATION Copy those
Create a .env and paste in the file

**Going back to app.py**
First, we need values to translate for that we must get the input values from the html form from the user - Text(that needs to be translated, then language that the user wants)
app.py: index_post route explanation # Read the values from the form
original_text = request.form['text']
target_language = request.form['language']
Now we need that text to be translated to target lang so we need to access the Azure Translate project for that we need KEY,ENDPOINT, LOCATION so we declare and access from the .env file that is created earlier # Load the values from .env
key = os.environ['KEY']
endpoint = os.environ['ENDPOINT']
location = os.environ['LOCATION']
We have the details where to has to reach initially //Let's say it as the Town Name and we need a Street and Door No. to reach to the correct address so we take path # Indicate that we want to translate and the API # version (3.0) and the target language
path = '/translate?api-version=3.0' # Add the target language parameter
target_language_parameter = '&to=' + target_language
This will be the complete address when combined # Create the full URL
constructed_url = endpoint + path + target_language_parameter
constructed_url= https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to='langname'

    # Set up the header information, which includes our
    # Azure subscription key
    headers = {
    	'Ocp-Apim-Subscription-Key': key,
    	'Ocp-Apim-Subscription-Region': location,
    	'Content-type': 'application/json',
    	'X-ClientTraceId': str(uuid.uuid4())
    }

    # Create the body of the request with the text to be
    # translated
    body = [{'text': original_text}]

    # Make the call using post
    translator_request = requests.post(
    	constructed_url, headers=headers, json=body)

    # Retrieve the JSON response
    translator_response = translator_request.json()

    # Retrieve the translation
    translated_text = translator_response[0]['translations'][0]['text']

    # Call render template, passing the translated text,
        # original text, and target language to the template
        return render_template(
            'results.html',
            translated_text=translated_text,
            original_text=original_text,
            target_language=target_language
        )

    In the results.html {{ original_text }} {{ translated_text }}  {{ target_language }} these will be dynamically updates based on the user inputs

**Things learnt in this project**

What is integrity attribute in the link tag?
The integrity property of the HTMLLinkElement interface is a string containing **inline metadata** that a **browser** can **use** to **verify** that a **fetched resource has been delivered without unexpected manipulation**.


