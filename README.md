# Heroku-AriaNG-21vianet. Heroku-AriaNG
Build AriaNG on Heroku, and upload to cloud drive when the file download completed.<br>

## Abuse Warning

**Heroku are actively banning this APP now.**<br>

**Your account will be SUSPENDED in highly possible.**<br>

## Catalog
* [Abuse Warning ](#abuse-warning-)
* [Improvement](#improvement)
* [Deploy by Docker Docker](#deploy-by-docker-recommend)
    * [Requirement](#requirement-)
    * [Steps](#steps)
* [Deploy by One-Click](#deploy-by-one-click)
* [Connect Cloud Drive](#optional-connect-cloud-drive)
* [FAQ](#faq)
* [Thanks](#thanks)

## Improvement

1. Rclone with 21vianet patch and Gclone mod.
2. Support mount double cloud drive.
3. Improve performance of the built-in Aria2c and Rclone.
4. Unpack(Beta) ZIP/RAR/7Z with password or sub-volume.
5. Fix some little issues in fork source.

## Deploy by Docker (Recommend)

FAQ: [Do I have to use Docker?](#do-i-have-to-use-docker)

### Requirement

* [Docker](https://www.docker.com/)
* [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli)
* [git](https://git-scm.com/)
* Ability to use terminal

### Steps

1. Run `heroku login` to login, then `heroku container:login` too.
2. Clone this repository and enter it. (PS: Please run `git config --global core.autocrlf false` before `git clone` if you are using Windows.)
3. Run `heroku apps:create APP_NAME` to create it.
4. Run `heroku config:set -a APP_NAME ARIA2C_SECRET=ARIA2_SECRET` and `heroku config:set -a APP_NAME HEROKU_APP_NAME=APP_NAME`.
5. Run `heroku container:push web -a APP_NAME` and `heroku container:release web -a APP_NAME`.
6. Run `heroku open -a APP_NAME` and it will open your browser to deployed instance.

## Deploy by One-Click

**Pay attention please: deploy by One-Click uses the Heroku built-in environment, that means your account might banned immediately.**


[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)

## Optional: Connect Cloud Drive

1. Setup Rclone by following [Rclone Docs](https://rclone.org/docs/)
You can find your config from there:

```
Windows: %userprofile%\.config\rclone\rclone.conf
Linux: $HOME/.config/rclone/rclone.conf
```
Optional: Using service account setup with [Gclone](https://github.com/donwa/gclone) to break Google Drive 750GB limit, or easier connect to folder or Team Drive by destination ID. First, open your rclone config and edit `service_account_file_path = /app/accounts/` as the SA json paths.

* Deploy by Docker: Create a new folder in your local repository root path after git clone, such as `/accounts/`, copy your SA json in it.<br>
* Deploy by One-Click: Fork this repository, and create a new folder in your forked repository, such as `/accounts/`, upload your SA json in it.

2. Your `rclone.conf` file, it should look like this:

```conf
[DRIVENAME A]
type = WHATEVER
client_id = WHATEVER
client_secret = WHATEVER
scope = WHATEVER
china_version = WHATEVER
token = WHATEVER

[DRIVENAME B]
type = WHATEVER
client_id = WHATEVER
client_secret = WHATEVER
scope = WHATEVER
china_version = WHATEVER
token = WHATEVER

others entries...
```

3. Set Rclone Config: Find the drive you want to use, and copy its `[DRIVENAME A] ...` to  `... token = ...` section, and replace all linebreaks with `\n`, and copy this TEXT.

* Deploy by Docker: Run `heroku config:set -a APP_NAME RCLONE_CONFIG=THAT_TEXT`.<br>
* Deploy by One-Click: Set `THAT_TEXT` in the `RCLONE_CONFIG` blank.

4. Set Rclone Upload Destination: `[DRIVENAME A]:[REMOVE PATH A]` means upload to `[REMOVE PATH A]` in `[DRIVENAME A]`. `[DRIVENAME A]` is the drive friendly name in rclone config, `[REMOVE PATH A]` is a remote path.

* Deploying by Docker: Run `heroku config:set -a APP_NAME RCLONE_DESTINATION=[DRIVENAME A]:[REMOVE PATH A]`.<br>
* Deploying by One-Click: Set `[DRIVENAME A]:[REMOVE PATH A]` in the `RCLONE_DESTINATION` blank.

5. If you mount a second cloud drive, Set `RCLONE_DESTINATION_2` same as step 4.

## FAQ

### Do I have to use Docker?
~~Not really, if you deployed previously and the APP is still working well, please enjoy it just like before.~~

~~If you want to deploy a new APP or rebuild the previously APP, I recommend using Docker to prevent account banned.~~

Heroku are actively prevent this APP. I recommend using Docker deploy a new APP or rebuild the previously APP to prevent account banned.

### Why it automatically stop after 30 minutes, and files were lost?
Heroku Free Dyno will idle when there is no incoming request within 30 minutes, and your files will be deleted, so use Rclone to breaking this or use Heroku Hobby Dyno.

By the way, the use of memory exceeds the limit of 512M for a long time, Heroku dyno also will idle.

### Why it still stop after 24 hours when i have used rclone?
Heroku-Aria APP will automatically make request to prevent idling when connect cloud drive with Rclone, but Heroku Dyno reset every 24 hours is inevitable.

### How to view upload process?
Go to Heroku Dashboard, and view application logs.

### How to edit rclone or aria2c config?
Open `on-complete.sh` and `aria2c.conf`, some global variables that can be edit, but believe me, the best parameters for best performance have been provided.

### How to delete files?
The file will be automatically deleted after the upload is complete. you can also delete the file by deleting the aria2 task.

### Can you provide more detailed configuration and deployment instructions
Nope. This README is enough.

## Thanks
Many thanks for maple3142/heroku-aria2c and P3TERX/aria2.conf.
