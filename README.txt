Creating the GRW Listing/Sorting/Filtering/API-Updating and Infinite loading app using IIS api
By Christopher van Heerden

This project is in alpha release.
If anything goes wrong, scroll to the top of the list and refresh the app
There is a glitch with infinite loading not knowing when to stop so duplicate JSON data is fetched and inserted into the dom
	This throws a duplicate key error - this to be sorted
There is a persisting CORS issue	
	Hostfile has been updated with 
		http://api-grw.local/
		http://grw.local/
	
	and the contents of web.config on http://api-grw.local/ is:
		<?xml version="1.0" encoding="UTF-8"?>
		<configuration>
			<system.webServer>
				<directoryBrowse enabled="true" />
				<httpProtocol>
					<customHeaders>
						<add name="Access-Control-Allow-Origin" value="*" />
					</customHeaders>
				</httpProtocol>
				<staticContent>
					<mimeMap fileExtension=".json" mimeType="application/json" />
				</staticContent>
				<defaultDocument>
					<files>
						<add value="performance_data.json" />
					</files>
				</defaultDocument>
			</system.webServer>
		</configuration>
		
		I use axios to do the post of the data 
Above are the issues with this app.

Below is the storyline of how i built the app since commitment date.		
https://vuejs.org/?
use vue cli-3
vue create my-project

Installed Windows featureIIS
in iis; point source to ./dist folder. NOTE: dist folder is populated by npm run build
https://github.com/vuejs/vue-devtools#vue-devtools - 
	source: https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd/related
	
used/installed https://desktop.github.com/  githubmail: 40839508+ChilliVanHot@users.noreply.github.com

run vue ui to create new test app and to see the user interface
install npm install --save-dev webpack webpack-cli

download and install https://www.iis.net/downloads/microsoft/url-rewrite
referenced https://www.linkedin.com/pulse/hosting-vue-js-spa-build-microsoft-iis-zainul-zain/
run npm run build


npm install --save axios
npm i vue-axios

npm install --save vue-lodash //sorting JSON
https://sabe.io/tutorials/using-axios-pull-data-rest-api

add :   <httpProtocol>
     <customHeaders>
       <add name="Access-Control-Allow-Origin" value="*" />
     </customHeaders>
   </httpProtocol> to api web.config to resolve CORS issue
   
   https://www.telerik.com/download/fiddler/fiddler4
   https://www.getpostman.com/apps
   
   Tonight - Wednesday - ADSL outage. Using mobile device tethering and 1Gig data for learning
   https://webpack.js.org/configuration/devtool/
-------------------------------------------------
   C:\Users\roaming\Documents\GitHub\GRW_ME\grw>npm run --dev
Scripts available in grw via `npm run-script`:
  serve
    vue-cli-service serve
  build
    vue-cli-service build
  lint
    vue-cli-service lint
-----------------------------------------------
https://codingthesmartway.com/vue-js-2-quickstart-tutorial-2017/

https://www.npmjs.com/package/vuetify

install https://www.npmjs.com/package/heroku to deploy this app to "live"
https://devcenter.heroku.com/articles/getting-started-with-nodejs#set-up

ON windows .cmd prompt run C:\Users\roaming\Documents\GitHub\GRW_ME\winner>heroku create grw-list-king-app
	after installing heroku-cli
	https://grw-list-king-app.herokuapp.com/ | https://git.heroku.com/grw-list-king-app.git

:\Users\roaming\Documents\GitHub\GRW_ME\winner>npm install express --save
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.4 (node_modules\fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.4: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","arch":"x64"})

+ express@4.16.3
updated 1 package and audited 18693 packages in 72.068s
found 0 vulnerabilities


C:\Users\roaming\Documents\GitHub\GRW_ME\winner>dir
 Volume in drive C is MAIN
 Volume Serial Number is BA46-7E16

 Directory of C:\Users\roaming\Documents\GitHub\GRW_ME\winner

2018/07/12  11:10 PM    <DIR>          .
2018/07/12  11:10 PM    <DIR>          ..
2018/07/11  10:22 PM               214 .gitignore
2018/07/12  09:25 AM    <DIR>          api
2018/07/11  10:22 PM                52 babel.config.js
2018/07/12  10:00 PM    <DIR>          dist
/  Building for production...
2018/07/12  11:10 PM    <DIR>          node_modules

2018/07/12  11:10 PM             1 085 package.json
2018/07/11  10:22 PM    <DIR>          public
2018/07/12  10:59 PM             3 523 README.txt
2018/07/12  02:39 PM    <DIR>          src
2018/07/12  09:13 AM               284 web.config
               7 File(s)        593 475 bytes
               7 Dir(s)  431 188 545 536 bytes free

C:\Users\roaming\Documents\GitHub\GRW_ME\winner>npm run build

> winner@0.1.0 build C:\Users\roaming\Documents\GitHub\GRW_ME\winner
> vue-cli-service build

I highly recommmend vue as a development framework - I love it.