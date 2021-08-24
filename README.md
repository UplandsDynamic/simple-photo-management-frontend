# Simple Photo Management - Frontend Client Component

## About

This is a demo/prototype of simple application built on web technologies to IPTC tag, search & image optimise digital photo libraries.

It has web frontend client that connects to a RESTful API backend. Data is stored in either a SQLite, mySQL or PostgreSQL (recommended) database.

The web client is built with React.js and the server backend is written in Python 3.7 using the Django framework.

**Important: Before scanning an image directory, please ensure you have backed up your data. This is a prototype and should be used with that in mind. Please do not put your only copy of digital photos at risk!**

## Support & project status

A one-off installation service for this app is available. Please contact spm@uplandsdynamic.com for further details.

The GPL licensed version of this project offered here is *not guaranteed* to be regularly maintained. It is made available here for demo/prototype purposes only, and should not be used in production (i.e. a "live" working environment) unless the administrator regularly patches project dependencies (i.e. PYPI & npm packages) with upstream security updates as and when released by vendors.

## Key technologies

Python 3.7, Django, Django-rest-framework, Javascript (ReactJS), HTML5, CSS3

## Key features

- Recursively scan directories for digital image files
- Add, remove & edit IPTC meta keyword tags from digital images via a web interface
- Easily select previously used IPTC meta keyword tags and add to digital images
- Automatically create a range of smaller (optimised) versions of each larger image (e.g. .jpg from a .tiff)
- Optimised versions named using a hash of the origin image file, to prevent duplication (note, the hash includes metadata, so if an identical looking image has had a change in metadata it is deemed 'different'.)
- Display & download optimised & resized versions of large images in the web interface
- Database IPTC tags associated with each image
- Search for & display digital images containing single IPTC tags or a combination of multiple tags
- Search and replace IPTC tags over all scanned image directories
- Restrict access to specific tags for specific usernames
- Switch between `light` and `dark` modes (by setting an environment variable)

## Live Demo

There is a live demo available here:

https://frontend.spm.webapps.uplandsdynamic.com

Login credentials are:

- Username: riker
- Password: z4Xd\*7byV\$xw

## Screenshots

![Screenshot 1](./meta/img/screenshot_01.png?raw=true)

## Usage Instructions

- Login to the web client using the superuser credentials you'd previously supplied.
- Click on the `+` button to scan the photo_directory for new original photos. By default, this action:
  - Recursively scans for digital images (.jpg, .tiff, .png)
  - Reads any IPTC keyword tags and adds them to the database.
  - The digital images are processed, with a range of image sizes automatically generated.
- Give it a few seconds and click the green refresh button (far left of the toolbar, beneath the page numbers). Images with no pre-existing IPTC keyword tags should be displayed (if any).
- To display images that do have tags, try typing a phrase into the search bar:
  - To search for tags containing a specifically defined phrase, enclose the phrase between quotation marks, e.g. "a phrase tag"
  - To search for images that contain a combination of multiple tags, separate search words or phrases with either a space, or a forward slash `/`.
- Clicking the button with the `tag` icon re-scans all images in photo_directory, adds any newly discovered images and recopies all IPTC keyword tags to the database. To simply add new images without re-copying the tags, use the `+` button instead.
- Clicking the button with the `broom` icon cleans the database of references to any processed images that no longer exist in the `media` directories or the origin image `photo_directory`.
- Clicking the button with the `slashed 'T'` icon removes all tags from the database which are not currently attached to any image (tag pruning). Note, this *does not* remove tags from an image, but simply cleans database records of unused tags.
- Clicking the button with the `swap` icon (left & right arrows) switches to `search & replace` mode, which allows replacement of an IPTC tag in all images with another:
  - Simply enter the term to search for in the upper `Search` field, the replacement tag in the `Replace` field, then click on the red button to `search & replace`.
  - To remove a tag from all images *without replacing it with an alternative*, type a dash character (also known as minus or tack) "`-`" into the `Replace` field. Then click on the red button to delete that tag from every image in the database.
- Add new tags to an image in one of two ways. These actions both write the new tag(s) to the metadata of the **ORIGINAL IMAGE** and to the database.:
  - By entering them in the input field, in the `Action` column. Separate multiple tags with a `/`.
  - By selecting from the list of previously used tags, that appears below the input field after you've begun to enter your tag. As you continue to type, this list resolves to display tags containing a sequence of characters that match your input.
- Processed images may be operated on in the following ways:
  - Clicking on a thumbnail image opens a larger resolution copy of that processed image. This may then be downloaded, via the toolbar appearing at the top of the image viewer. Note, this is __not__ the *original* image, but rather a higher resolution version of the processed image.
  - The thumbnail images themselves have a mini-toolbar of buttons beneath them:
    - `Anticlockwise arrow` rotates the processed images (thumbnail and the generated larger versions up to 1080px wide) anticlockwise.
    - `Clockwise arrow` rotates the processed images (thumbnail and the generated larger versions up to 1080px wide) clockwise.
    - `Refresh symbol` reprocesses that image record. Processed images (thumbnail and larger versions up to 1080px wide) are generated and tags coped to them from the original image. This is useful if, for example, a processed image has been accidentally deleted, or corrupted for some reason.
- Occasionally, the user may encounter errors during tag writing, due to problematical or corrupted preexisting metadata. In that case, a `mod_lock` flag is set in the database and the metadata of the original image is no longer allowed to be modified by the system until this flag has been removed. Clicking on the `red button with a crossed 'T' & padlock icon` will bulk remove *ALL* metadata from *ALL* original images that have the `mod_lock` flag set. Once the problematical metadata on an image has been deleted, the `mod_lock` flag is removed. Instigating this function should obviously be done with caution.
- To restrict access to specific tags for specific usernames, log in to the Django admin dashboard (typically found at `http://your_api_domain.tld/admin`). After logging in, navigate to the `SPM_APP > Photo Tag` page. Once there, you may select tags and assign them to specific usernames.
Note: the users would need to have already been created - however they should have *no* administrative privileges, nor be assigned to the `administrators` group, since there would otherwise be no point in providing access to specific tags, because users in the `administrators` group have read/write permissions on all records in any case. Non-administrator usernames have access to *only those tags which have been assigned to those usernames*.

## Installation (on Linux systems)

__Below are basic steps to install and run a demonstration of the app on an Linux Ubuntu 20.04 server. They do not provide for a secure installation, such as would be required if the app was publicly available. Steps should be taken to security harden the environment if using in production.__

### Brief installation instructions

- Clone repository to an app source directory of your choice, e.g. `git clone https://github.com/Aninstance/simple-photo-management-frontend.git`.
- Make web root directory, e.g. `mkdir -p /var/www/html/spm-frontend`.
- Move into the `react` directory of the cloned directory and install the required node packages. Ensure a current version of NPM is installed and active on your system - it's recommended to install NVM (Node Version Manager) - which allows multiple node versions to be installed - to avoid changing a pre-installed version that may be required by other software or packages on your system.
- Install npx on your system if not already installed, e.g.: `npm install -g npx`.
- Install the packages by running `npm install --legacy-peer-deps`.
- In the event of any vulnerabilities being flagged up, repeat `npm install --legacy-peer-deps` to see if that resolves the issue. Do this *first, before* resorting to running `npm audit fix`.
- Configure the app environment by coping the `.env.production_DEFAULT` file to `.env.production` and editing according to your requirements.
- Build the app, e.g.: `npm run build:production`.
- Copy the built app into its web directory, e.g. `cp -a build/. /var/www/html/spm-frontend/;` `cp -a /var/www/html/spm-frontend/static. /var/www/html/spm-frontend/;`.
- Recursively change ownership of the `spm-frontend` directory to your web server user.
- Configure your web server to serve the app from your web directory (this is straight forward, but outwith the scope of this document. If you need further help, please check your web server's documentation).

### Update Instructions

- Ensure you backup a copy of changes to your environment configuration - make a copy of your `.env.production` file *outside* of your cloned directory.
- From the build directory, run `git pull`.
- Remove existing node modules, e.g.: `rm -rf node_modules`.
- Clear the cache, e.g.: `npm cache clear --force`
- Install updated modules, e.g.: `npm install --legacy-peer-deps`.
- In the event of any vulnerabilities being flagged up, run `npm audit fix`.
- Rebuild the project, e.g.: `npm run build:production`.
- Once the app has been built, copy to the web directory: `cp -a build/. /var/www/html/spm-frontend/;` `cp -a /var/www/html/spm-frontend/static/. /var/www/html/spm-frontend/;`.
- Recursively change ownership of the `spm-frontend` directory to your web server user.
- Restart your web server.

Note: The above guide is not definitive and is intended for users who know their way around Ubuntu server and Django.

*Users would need to arrange database backups and to secure the application appropriately when used in a production environment.*

## Development Roadmap

- ~~Automated display of tag suggestions (based on real-time character matching & most used) when adding IPTC tags to an image~~ [Complete]
- Enhancement of `clean` to facilitate deletion of processed image files & thumbnails (rather just database entries) when origin image no longer exists
- Tag suggestions based on facial recognition
- Expose switching between `light` and `dark` modes on the UI rather than requiring setting of environment variable
- ~~Remove tags from the used-tag list if no longer used for any images~~ [Complete]
- ~~Add ability to delete tags after search (currently limited to replace with an alternative tag)~~ [Complete]

## Authors

- Dan Bright (Uplands Dynamic), dan@uplandsdynamic.com
