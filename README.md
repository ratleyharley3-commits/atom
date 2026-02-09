File Icons
==========

[![Build status: TravisCI](https://img.shields.io/travis/com/file-icons/atom)](https://app.travis-ci.com/github/file-icons/atom/builds)
[![Latest package version](https://img.shields.io/github/v/release/file-icons/atom?label=apm&color=brightgreen)](https://github.com/file-icons/atom/releases/latest)

File-specific icons in Atom for improved visual grepping.

<img alt="Icon previews" width="850" src="https://raw.githubusercontent.com/file-icons/atom/6714706f268e257100e03c9eb52819cb97ad570b/preview.png" />

Supports the following core packages:

* [`tree-view`](https://atom.io/packages/tree-view)
* [`tabs`](https://atom.io/packages/tabs)
* [`fuzzy-finder`](https://atom.io/packages/fuzzy-finder)
* [`find-and-replace`](https://atom.io/packages/find-and-replace)
* [`archive-view`](https://atom.io/packages/archive-view)

An API is offered for packages not listed above. See the [integration steps][13] for more info.


Installation
------------
Open **Settings** → **Install** and search for `file-icons`.

Alternatively, install through command-line:

	apm install --production file-icons


Customisation
-------------
Everything is handled using CSS classes. Use your [stylesheet][1] to change or tweak icons.

Consult the package stylesheets to see what classes are used:

* **Icons:**   [`styles/icons.less`](./styles/icons.less)
* **Colours:** [`styles/colours.less`](./styles/colours.less)
* **Fonts:**   [`styles/fonts.less`](./styles/fonts.less)


#### Icon reference
* [**File-Icons**](https://github.com/file-icons/icons/blob/master/charmap.md) 
* [**FontAwesome 4.7.0**](https://fontawesome.com/v4.7.0/cheatsheet/)
* [**Mfizz**](https://github.com/file-icons/MFixx/blob/master/charmap.md)
* [**Devicons**](https://github.com/file-icons/DevOpicons/blob/master/charmap.md)


### Examples ####################################################################

#### Resize an icon
~~~less
.html5-icon:before{
	font-size: 18px;
}

// Resize in tab-pane only:
.tab > .html5-icon:before{
	font-size: 18px;
	top: 3px;
}
~~~


#### Choose your own shades of orange
~~~css
.dark-orange   { color: #6a1e05; }
.medium-orange { color: #b8743d; }
.light-orange  { color: #cf9b67; }
~~~


#### Bring back PHP's blue-shield icon
~~~css
.php-icon:before{
	font-family: MFizz;
	content: "\f147";
}
~~~


#### Assign icons by file extension
The following examples use [attribute selectors][12] to target specific pathnames:

~~~css
.icon[data-name$=".js"]:before{
	font-family: Devicons;
	content: "\E64E";
}
~~~


#### Assign icons to directories
~~~less
.directory > .header > .icon{
	&[data-path$=".atom/packages"]:before{
		font-family: "Octicons Regular";
		content: "\f0c4";
	}
}
~~~



Troubleshooting
---------------

<a name="error-after-installing"></a>
#### I see this error after installing:
> _"Cannot read property 'onDidChangeIcon' of undefined"_

A restart is needed to complete installation. Reload the window, or restart Atom.

If this doesn't help, [please file an issue][7].



<a name="npm-error-when-installing"></a>
#### Installation halts with an `npm` error:
> _npm ERR! cb() never called!_

There might be a corrupted download in your local cache.
Delete `~/.atom/.apm`, then try again:

~~~shell
rm -rf ~/.atom/.apm
apm install --production file-icons
~~~



<a name="an-icon-has-stopped-updating"></a>
#### An icon has stopped updating:
It's probably a caching issue. Do the following:

1. Open the command palette: <kbd>Cmd/Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd>
2. Run `file-icons:clear-cache`
3. Reload the window, or restart Atom



<a name="ruby-files-look-weird"></a>
#### Ruby files are showing the [wrong font][14]:
If [`language-ethereum`][15] is installed, remove it.
This is a [known issue][16] with the package, which is no longer maintained.
For Solidity support, use [`linter-solidity`][17] or [`language-solidity`][18] instead.

If `language-ethereum` *isn't* installed, please [follow these steps][19] and file an issue.



<a name="the-tree-views-files-are-borked"></a>
#### The tree-view's files are borked and [look like this][6]:
If you haven't restarted Atom since upgrading to [File-Icons v2][v2.0], do so now.

If restarting doesn't help, your stylesheet probably needs updating. See below.



<a name="my-stylesheet-has-errors-since-updating"></a>
#### My stylesheet has errors since updating:
As of [v2.0][], classes are used for displaying icons instead of mixins. Delete lines like these from your stylesheet:

~~~diff
-@import "packages/file-icons/styles/icons";
-@import "packages/file-icons/styles/items";
-@{pane-tab-selector},
.icon-file-directory {
	&[data-name=".git"]:before {
-		.git-icon;
+		font-family: Devicons;
+		content: "\E602";
	}
}
~~~


Instead of `@pane-tab…` variables, use `.tab > .icon[data-path]`:

~~~diff
-@pane-tab-selector,
-@pane-tab-temp-selector,
-@pane-tab-override {
+.tab > .icon {
 	&[data-path$=".to.file"] {
 		
 	}
}
~~~


These CSS classes are no longer used, so delete them:

~~~diff
-.file-icons-force-show-icons,
-.file-icons-tab-pane-icon,
-.file-icons-on-changes
~~~


#### It's something else.
Please [file an issue][7]. Include screenshots if necessary.



Integration with other packages
-----------------------------------------------------------------------------------
If you're a package author, you can integrate File-Icons using Atom's services API.

First, add this to your `package.json` file:

```json
"consumedServices": {
	"file-icons.element-icons": {
		"versions": {
			"1.0.0": "consumeElementIcons"
		}
	}
}
```

Secondly, add a function named `consumeElementIcons` (or whatever you named it) to your package's main export:

```js
let addIconToElement;
module.exports.consumeElementIcons = function(func){
	addIconToElement = func;
};
```

Then call the function it gets passed to display icons in the DOM:

```js
let fileIcon = document.querySelector("li.file-entry > span.icon");
addIconToElement(fileIcon, "/path/to/file.txt");
```

The returned value is a [`Disposable`][10] which cl
