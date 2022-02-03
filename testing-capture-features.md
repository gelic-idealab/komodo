How to manually test capture features:

### Create a Capture

* Log in in spectator mode, preferably on a PC
* Optionally connect with a separate VR client
* In spectator mode...
	* Open Settings > Instuctor Menu
	* Press Start Capture
* In VR or spectator mode...
	* Perform the desired actions
* In spectator mode, press Stop Capture

### Check the Capture Data

* Access your captures folder. 
	* Institution-hosted: contact your Komodo administrator.
	* Local, minimal instance: look in your komodo-relay folder.
* Open the folder <session num>/<timestamp>
* Option 1: Search the text manually with a text editor.
  * Rename data to data.json
  * Open data.json in a text editor
  * (Recommended) Format the JSON so that it has line breaks and indentation. This should make the "Find" feature run faster
  * Use Ctrl F or a Find feature to look for the desired messages.
* Option 2: Place the JSON in a JS editor with a console, then search the data with JS.
  * TODO

See also:
* Guide to Komodo Messages: TODO
