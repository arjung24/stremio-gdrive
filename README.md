
# stremio-gdrive

![image](https://user-images.githubusercontent.com/38104354/114273627-6f3c4900-9a38-11eb-8052-f05f5f414e84.png)
![image](https://user-images.githubusercontent.com/38104354/114273632-74999380-9a38-11eb-9e7d-4ab6438ef445.png)

## Instructions:

There are two ways to go about:

* **Method 1** is hard and long but might give you better performance and you need to make your own google cloud project thingy. 
* **Method 2** relies on [rclones](https://rclone.org/) google cloud project saving you the burden of having to create your own project i.e. you get to skip the next 15 steps.

### Method 1 starts from here

1. Go to https://console.cloud.google.com/projectcreate
2. Name the project whatever you want and click create. 
3. Next to "Google Cloud Platform," click the Down arrow ![image](https://user-images.githubusercontent.com/38104354/113966809-6d626200-984d-11eb-96df-ca21e06b44c1.png) and select your project.
4. At the top-left corner, click the hamburger menu icon: ![image](https://user-images.githubusercontent.com/38104354/113966919-9a167980-984d-11eb-94c9-44d0e329a250.png) Click APIs & Services > Credentials.
5. Click Configure Consent Screen. The "OAuth consent screen" screen appears.
6. Set user type to external and click create.
7. Fill in the required details, you can give your own email as dev and support email. Click save and continue.
8. For the scopes page leave it as it is and click save and continue.
9. Now add yourself (your email id) as the test user by pressing add users. Click save and continue.
10. Now on your left, under API & Services, you will see Dashboard. Click on it and then click on Enable APIs and Services.
11. Search for Google Drive and click on the result that says Google Drive API and then click on Enable.
12. Now do step 4 again.
13. Click Create Credentials > OAuth client ID.
14. Choose application type as Desktop, name it whatever you want and click create.
15. The newly created credential appears under "OAuth 2.0 Client IDs." Click the download button to the right of the newly-created OAuth 2.0 Client ID. We will get a client_secret.json file, we will need this for the next section.

### Method 2 starts from here

16. Use the colab notebook to easily obtain the token and the required code for the cloudflare proxy: 

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ssnjrthegr8/stremio-gdrive/blob/main/Get%20Token%20and%20CF%20Proxy%20Code.ipynb)

17. Run the cell of the respective method number you chose. Just press the circular play button beside the rectangular box.

18. Login with your google account. Copy the js code and copy the token string for later when we deploy the heroku backend, the token should look something like this:

`{"token": "jhgdfgdhgfh", "refresh_token": "1//sdhgbfdhghd", "token_uri": "https://oauth2.googleapis.com/token", "client_id": "hsdgfjhgfsd.apps.googleusercontent.com", "client_secret": "gfsdfsdgf", "scopes": ["https://www.googleapis.com/auth/drive"]}`

19. Go to https://dash.cloudflare.com/ log in or sign up.
20. Open the Workers option.
21. Create your sub-domain or continue if already done.
22. Select the Free Plan. Click on Create a Worker. You can rename the workers at top of the page.
23. In the `Script{}` box you will see some code, delete all that and paste the js code that you obtained in step 18 there.

### Deploying to heroku:

[![Deploy to Heroku](https://www.herokucdn.com/deploy/button.png)](https://heroku.com/deploy?template=https://github.com/ssnjrthegr8/stremio-gdrive.git)

1. Hit the deploy button.
2. Paste the token string in the token field.
3. Copy your cloudflare worker url and paste it in the cf proxy url field.
4. Hit deploy.

### Adding addon to Stremio:

1. Get the url of the heroku app you deployed. Add `/manifest.json` to the end. For example if my herokuapp url is `https://stremiogdrive.herokuapp.com/` add `/manifest.json` to the end to get: `https://stremiogdrive.herokuapp.com/manifest.json`. Copy this url.
2. Go to https://staging.strem.io/ and log into your stremio account. Press the puzzle icon on the top right.
3. On your left you will see a search box with the text "Search Addons", paste the url you got in step 1 in the search box. A prompt will open up, press install.

## Issues / Limitations / Explanations
* Apps hosted on heroku sleeps after 30 mins of inactivity. So after 30 mins of inactivity the addon needs about 30 secs to start back up and then some time to fetch the results. If you use something like [Kaffeine](https://kaffeine.herokuapp.com/) you can overcome this as Kaffeine pings your app every 30 mins. Downside is that you will spend extra dyno hours. Free dynos have about 550 hours of runtime per month which is about 22 days.
* Google api is not very fast so if you have a ton of results it might take 10 - 15 secs to load.
* Cloudflare workers is used as a proxy to stream files from google drive since it is really fast.
* I am an amateur and I did this to the best of my ability. If you find flaws feel free to post an issue.

### **For the sake of security please don't share your herokuapp url or your cloudflare workers proxy url. They can be used to get files from your drive.**

## Credits
1. To the [libDrive project](https://github.com/libDrive/cloudflare) for one part of the cloudflare js code.
2. To [alx-xlx go index repo](https://github.com/alx-xlx/goindex/blob/master/goindex.js) for the other part of the cloudflare js code.
