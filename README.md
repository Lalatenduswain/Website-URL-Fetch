# Website URL Fetch

This bash script is designed to crawl a website, traverse its links, and extract URLs (with included filtering) that will be saved to a generated text file.

## Usage

1. Save the script to your preferred location on the device by [Downloading](https://github.com/Lalatenduswain/Website-URL-Fetch/archive/refs/heads/master.zip) it."
2. Ensure that `wget` is installed on your device beforehand.

    To verify if it is installed, try executing the `wget` command. If you are using a Mac or Linux operating system, wget is likely already pre-installed; however, if the command fails to run, it may not be correctly added to your PATH environment variable."

    If you are running Windows:

    1. Download the lastest `wget` binary for windows from [https://eternallybored.org/misc/wget/](https://eternallybored.org/misc/wget/)

        The download is available as a zip with documentation, or just an exe. I'd recommend just the exe.
    2. If you downloaded the zip, extract all (if windows built in zip utility gives an error, use 7-zip). In addition, if you downloaded the 64-bit version, rename the `wget64.exe` file to `wget.exe`
    3. Move `wget.exe` to `C:\Windows\System32\`

3. Ensure the version of `grep` on your computer supports `-E, --extended-regexp`. To check for support, run `grep --help` and look for the flag. To check the installed version, run `grep -V`.

4. Open Git Bash, Terminal, etc. and set execute permissions for the `fetchurls.sh` script:

    ```shell
    chmod +x /path/to/script/fetchurls.sh
    ```

5. Enter the following to run the script:

    ```shell
    ./fetchurls.sh [OPTIONS]...
    ```

    Alternatively, you may execute with either of the following:

    ```shell
    sh ./fetchurls.sh [OPTIONS]...

    # -- OR -- #

    bash ./fetchurls.sh [OPTIONS]...
    ```

If you do not pass any options, the script will run in interactive mode.

If the domain URL requires authentication, you must pass the username and password as flags; you are not prompted for these values in interactive mode.

## Options

You may pass options (as flags) directly to the script, or pass nothing to run the script [in interactive mode](#interactive-mode).

### domain

- Usage: `-d`, `--domain`
- Example: `https://example.com`
- Final Script: `./fetchurls.sh -d https://www.domain.com`

The fully qualified domain URL (with protocol) you would like to crawl.

Ensure that you enter the correct protocol (e.g. `https`) and subdomain for the URL or the generated file may be empty or incomplete. The script will automatically attempt to follow the first HTTP redirect, if found. For example, if you enter the incorrect protocol (`http://...`) for `https://www.lalatendu.info`, the script will automatically follow the redirect and fetch all URLs for the correct HTTPS protocol.

The domain's URLs will be successfully spidered as long as the target URL (or the first redirect) returns a status of `HTTP 200 OK`.

### location

- Usage: `-l`, `--location`
- Default: `~/Desktop`
- Example: `/c/Users/username/Desktop`

The location (directory) where you would like to save the generated results.

If the directory does not exist at the specified location, as long as the rest of the path is valid, the new directory will automatically be created.

### filename

- Usage: `-f`, `--filename`
- Default: `domain-topleveldomain`
- Example: `example-com`

The desired name of the generated file, without spaces or file extension.

### exclude

- Usage: `-e`, `--exclude`
- Default: [See the default list of excluded file extensions](#excluded-files)
- Example: `"css|js|map"`

Pipe-delimited list of file extensions to exclude from results.

To prevent excluding files matching the default list of file extensions, simply pass an empty string `""`

### sleep

- Usage: `-s`, `--sleep`
- Default: `0`
- Example: `2`

The number of seconds to wait between retrievals.

### username

- Usage: `-u`, `--username`
- Example: `marty_mcfly`

If the domain URL requires authentication, the username to pass to the wget command.

If the username contains space characters, you must pass inside quotes. This value may only be set with a flag; there is no prompt in interactive mode.

### password

- Usage: `-p`, `--password`
- Example: `thats_heavy`

If the domain URL requires authentication, the password to pass to the wget command.

If the password contains space characters, you must pass inside quotes. This value may only be set with a flag; there is no prompt in interactive mode.

### non-interactive

- Usage: `-n`, `--non-interactive`

Allows the script to run successfully in a non-interactive shell.

The script will utilize the default [`--location`](#l-location) and [`--filename`](#f-filename) settings unless the respective flags are explicitely set.

### ignore-robots

- Usage: `-i`, `--ignore-robots`

Ignore robots.txt for the domain.

### wget

- Usage: `-w`, `--wget`

Show wget install instructions. The installation instructions may vary depending on your computer's configuration.

### version

- Usage: `-v`, `-V`, `--version`

Show version information.

### troubleshooting

- Usage: `-t`, `--troubleshooting`

Outputs received option flags with their associated values at runtime for troubleshooting.

### help

- Usage: `-h`, `-?`, `--help`

Show the help content.

## Interactive Mode

If you do not pass the --domain flag, the script will run in interactive mode and you will be prompted for the unset options.

First, you will be prompted to enter the full URL (including HTTPS/HTTP protocol) of the site you would like to crawl:

```shell
Fetch a list of unique URLs for a domain.

Enter the full domain URL ( http://example.com )
Domain URL:
```

You will then be prompted to enter the location (directory) of where you would like the generated results to be saved (defaults to Desktop on Windows):

```shell
Save file to directory
Directory: /c/Users/username/Desktop
```

Next, you will be prompted to change/accept the name of the generated file (simply press enter to accept the default filename):

```shell
Save file as
Filename (no file extension, and no spaces): example-com
```

Finally, you will be prompted to change/accept the default list of excluded file extensions (press enter to accept the default list):

```shell
Exclude files with matching extensions
Excluded extensions: bmp|css|doc|docx|gif|jpeg|jpg|JPG|js|map|pdf|PDF|png|ppt|pptx|svg|ts|txt|xls|xlsx|xml
```

The script will crawl the site and compile a list of valid URLs into a new text file. When complete, the script will show a message and the location of the generated file:

```shell
Fetching URLs for example.com

Finished with 1 result!

File Location:
/c/Users/username/Desktop/example-com.txt
```

If a file of the same name already exists at the location (e.g. if you previously ran the script for the same URL), **the original file will be overwritten**.

## Excluded Files and Directories

The script, by default, filters out many file extensions that are commonly not needed.

The list of file extensions can be passed via the [`--exclude` flag](#exclude), or provided via the interactive mode.

### Excluded Files

- `.bmp`
- `.css`
- `.doc`
- `.docx`
- `.gif`
- `.jpeg`
- `.jpg`
- `.JPG`
- `.js`
- `.map`
- `.pdf`
- `.PDF`
- `.png`
- `.ppt`
- `.pptx`
- `.svg`
- `.ts`
- `.txt`
- `.xls`
- `.xlsx`
- `.xml`

### Excluded Directories

In addition, specific site (including WordPress) files and directories are also ignored.

- `/wp-content/uploads/`
- `/feed/`
- `/category/`
- `/tag/`
- `/page/`
- `/widgets.php/`
- `/wp-json/`
- `xmlrpc`

## Advanced Usage

The script should filter out most unwanted file types and directories; however, you can edit the regular expressions that filter out certain pages, directories, and file types by editing the `fetchUrlsForDomain()` function within the `fetchurls.sh` file.

**Warning**: If you're not familiar with [grep](https://man7.org/linux/man-pages//man1/grep.1.html) or regular expressions, you can easily break the script.
