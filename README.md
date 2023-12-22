# StreamDeck VMix API Request Plugin

A Plugin for the Elgato Stream Deck for sending VMix commands using API requests.

Additionally, the plugin can poll a (different, if required) API at a
pre-determined interval in order to update the key icon to reflect external
changes in state.

### Features:
* Specifying HTTP Method, Headers, and Body if applicable
* Parsing of request response to set key icon
* Periodic Polling of VMix status API
* Polling of status on startup, configuration changes, and upon initial display
  (when changing to a streamdeck profile with the plugin visible)

### Response parsing supports:
* Path to an XML field in the VMix status response, and expected matching value
* See below for typical XML response from VMix

### Setting icons
* Note, no button icon must be set in the Streamdeck App. If you've allocated a button icon yourself then the API cannot change it. Click on the "Down" arrow on the button image and select "Reset to Default". The icon set by the API should then show up.

## Getting Started on plugin editing
Getting started with Stream Deck.

The fastest way to get started with Stream Deck is using the plugin template.
1.  Stream Deck Plugin Template
    * Checkout the plugin template from 
 `git clone https://github.com/elgatosf/streamdeck-plugin-template`

2. Refactor the Unique Identifier
   * Rename the folder `com.elgato.template.sdPlugin` using i.e. `com.example-url.plugin-name.sdPlugin`. Replace any reference to `com.elgato.template` in the manifest.json and app.js.
3. Javascript Libraries
   * The Javascript SDK communicates with the Stream Deck websocket and listens for events. Clone this library into the plugin folder or add it as a git submodule.
   * Clone
`git clone https://github.com/elgatosf/streamdeck-javascript-sdk src/my.domain.plugin-name/libs`

   * Add Submodule
`git submodule add https://github.com/elgatosf/streamdeck-javascript-sdk src/my.domain.plugin-name/libs`
4. Add the Plugin to Stream Deck
   * Create a symbolic link of the plugin's folder inside of the Stream Decks Plugins folder.
   * Windows SymLink
(Note: this works inside the cmd, not on PowerShell.
 %cd% gets the full absolute path to the plugin folder)
`mklink /D C:\Users\%USERNAME%\AppData\Roaming\Elgato\StreamDeck\Plugins\com.example.my-plugin.sdPlugin %cd%\src\com.example.my-plugin.sdPlugin`
   * macOS SymLink
(Using \$(pwd) to get the full absolute path to the plugin folder)
`ln -s $(pwd)/src/com.example.my-plugin.sdPlugin ~/Library/Application\ Support/com.elgato.StreamDeck/Plugins/`
5. Debugging
   * Set the `html_remote_debugging_enabled` flag and restart Stream Deck. A list of plugins available for debugging are now available from a browser at `http://localhost:23654/`. Refresh the plugin or property inspector at any time by reloading its page (Changes to the manifest.json will still require restarting Stream Deck).
   * With debugging enable, you can also debug the property inspector (user interface) of your plugins. 

   * macOS Debugging:
`defaults write com.elgato.StreamDeck html_remote_debugging_enabled -bool YES`
   * Windows Debugging: 
On Windows, add a DWORD `html_remote_debugging_enabled` with value 1 in the registry `@HKEY_CURRENT_USER\Software\Elgato Systems GmbH\StreamDeck`.

6. Build the Plugin
*  Everything is now configured to build a Stream Deck plugin!


## Typical VMix XML status response

Send `http://127.0.0.1:8088/api`

```
  <vmix>
  <version>23.0.0.57</version>
  <edition>Basic</edition>
  <inputs>
  <input key="695b6d65-3548-47f9-9a08-bcddf967265e" number="1" type="Blank" title="Blank" shortTitle="Blank" state="Paused" position="0" duration="0" loop="False">Blank</input>
  <input key="bb198a82-d7a0-463e-b4c0-c97187b784cc" number="2" type="Blank" title="Blank" shortTitle="Blank" state="Paused" position="0" duration="0" loop="False">Blank</input>
  </inputs>
  <overlays>
  <overlay number="1"/>
  <overlay number="2"/>
  <overlay number="3"/>
  <overlay number="4"/>
  <overlay number="5"/>
  <overlay number="6"/>
  </overlays>
  <preview>2</preview>
  <active>1</active>
  <fadeToBlack>False</fadeToBlack>
  <transitions>
  <transition number="1" effect="Fade" duration="500"/>
  <transition number="2" effect="Merge" duration="1000"/>
  <transition number="3" effect="Wipe" duration="1000"/>
  <transition number="4" effect="CubeZoom" duration="1000"/>
  </transitions>
  <recording>False</recording>
  <external>False</external>
  <streaming>False</streaming>
  <playList>False</playList>
  <multiCorder>False</multiCorder>
  <fullscreen>False</fullscreen>
  <audio>
  <master volume="100" muted="False" meterF1="0" meterF2="0" headphonesVolume="100"/>
  </audio>
  </vmix>
```  