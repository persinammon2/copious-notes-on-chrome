# copious-notes-extension
I did not find an equivalent extension, so I made it.
A Chrome extension for adding private notes to a GitHub repository.


## How to Run

The end workflow is run `npm run build` from the root of the project, go to `chrome://extensions` with Developer Mode activated,
Load unpacked the `dist` folder, pin the new extension to the Chrome toolbar, and use the refresh and errors buttons of the extension
on the `chrome://extensions` page for development. Remember, do not touch the files in `dist` folder. Any changes will be overwritten after every build script run, make all changes in the `public` folder.

Also, if for some reason in development you end up without the `node_modules` generated by `npm`, you can run `npm clean-install` then the previous instructions.


## TypeScript

I experimented with using [TypeScript](https://www.typescriptlang.org/docs/handbook/typescript-from-scratch.html) for the first time. Google Developer documentation does not have examples with TypeScript (all their examples [here](https://github.com/GoogleChrome/chrome-extensions-samples/tree/main)), but they do offer the package `@types/chrome` to developers willing to try it. I used a tutorial on Medium to set up a workflow, with the final extension code being exported to the `dist` folder.

Despite the overhead, I'm a huge fan of always using TypeScript as a best practice.

Instructions to Install:

- Install Node.js and npm, check installation with `node -v && npm -v`.
- Run `npm init -y`.
- Install required dependencies under the `--save-dev` flag. `--save-dev` saves packages under `devDependencies` in `package.json`, so the packages are not installed in production environment (keep build minimal). This flag is not important in this case, as what is ending up being used in production is the generated vanilla JS in `dist`.


```
npm install --save-dev webpack webpack-cli &&
npm install --save-dev copy-webpack-plugin &&
npm install --save-dev typescript ts-loader &&
npm install --save-dev @types/chrome
```
- Do extra setup detailed [here](https://betterprogramming.pub/creating-chrome-extensions-with-typescript-914873467b65).


Super credits to the Medium article, very cool. I also found a second Medium [article](https://medium.com/@steveruiz/using-a-javascript-library-without-type-declarations-in-a-typescript-project-3643490015f3) for backwards compatibility for adding node modules with no TypeScript files at all to a project.


## Notes

- Similar to how Jupyter GRAFITTI allows for in-line voice messages on Jupyter notebooks.
- Chrome extensions run in a separate, sandboxed execution environment and interact with the Chrome browser. They "extend" the browser by using APIs to modify browser behavior and access web content (excerpted from [here](https://betterprogramming.pub/creating-chrome-extensions-with-typescript-914873467b65)).
- An optional manifest key used to specify a javascript file as the extension service worker. A service worker is a background script that acts as the extension's main event handler. For more information, visit the more comprehensive introduction to service workers...Extension service workers are an extension's central event handler...An extension service worker is loaded when it is needed, and unloaded when it goes dormant. Once loaded, an extension service worker generally runs as long as it is actively receiving events, though it can shut down. Like its web counterpart, an extension service worker cannot access the DOM, though you can use it if needed with offscreen documents. [Source](https://developer.chrome.com/docs/extensions/mv3/manifest/background/)

```
  "permissions": [
    "storage",
    "contextMenus", // add action item in 'right click' browser context menu - necessary
    "downloads", // nice to have download of highlights to share to people
    "extensionTypes", //unsure about this
    "types", //unsure about this
    "commands" //optional to make a keyboard shortcut
  ],
```
- `service-worker.ts` -> `background.js`


# Functionality

- User highlights text -> either in the right click window or right away a button by mouse appears with tag "Annotate text block/code section." -> Click "Annotate..." -> Pop up with text box appears where can write notes (in markdown?) -> Click save on bottom right of that text box. -> key-value of that location/set of words on that page to the user made notes is saved to dictionary per page.
- Where user previously saved note, the text is highlighted. 
- User goes to a page, and goes to the tab where can see all notes for the page and can access directly.
- When on search page, the tab shows all pages that have highlights on them.

- Download data to export for others to view (no real-time collaboration feature).

## Sources

https://betterprogramming.pub/creating-chrome-extensions-with-typescript-914873467b65

https://www.linkedin.com/pulse/vanilla-javascript-concepts-you-need-know-before-learning#:~:text=Vanilla%20JavaScript%20is%20a%20term,any%20plugins%20or%20other%20libraries.

https://github.com/GoogleChrome/chrome-extensions-samples/tree/main

https://developer.chrome.com/docs/extensions/mv3/getstarted/tut-tabs-manager/

https://developer.chrome.com/docs/extensions/mv3/architecture-overview/#async-sync