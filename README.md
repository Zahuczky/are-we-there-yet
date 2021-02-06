# Are We There Yet?

Web client for Deschtimes group status.

## Local Development

```
npm run dev
```

## Deploying the Application

You have multiple options to deploy the app.

### Deploying to Vercel

1. Fork this repository.
2. [Login to Vercel.](https://vercel.com/) GitHub, GitLab, Bitbucket, and plain e-mail are all supported. Create a new project.
3. When asked about the directory containing the project source code, the default is fine, hit **Continue**.
4. Switch **FRAMEWORK PRESET** to `Svelte`.
5. Under **Build & Output Settings**:
    - Toggle _Override_ for `BUILD COMMAND` and set it to `npm run build:vercel`
    - Toggle _Override_ for `OUTPUT DIRECTORY` and set it to `build`
6. Under **Environment Variables**:
    - Set **NAME** to `TOKEN`
    - Set **VALUE** to your group token from Deschtimes
7. Click **Add** and then **Deploy**.
8. Under your newly made project, go to **Settings** > **Git**, and scroll down to **Deploy Hooks**.
9. Create a Hook with a descriptive name (`Deschtimes` is suggested) and type in `main` for your `Git Branch Name`, click **Create Hook**. **Copy** the newly added webhook URL and do not share it with anyone else.
10. Log in to Deschtimes and go to your Group page. Under **Webhooks**, click **Manage**.
11. Click **Add Webhook**. Use a descriptive name like `Vercel`, and paste the URL you obtained from step 9. Leave the platform as **Generic**, press **Create Webhook**.

Your application will be statically generated every time your group's projects are updated on Deschtimes.

### Deploying to Netlify

**Currently broken**, please use another deployment method for now.

1. Fork this repository.
2. [Login to Netlify](https://app.netlify.com/signup). GitHub, GitLab, Bitbucket, and plain e-mail are all supported.
3. Under project type, select **Web Application** and **Get started**.
4. Connect to a Git provider.
5. Select your fork of this repository.
6. Under **Basic build settings**, click **Show advanced**.
7. Under **Advanced build settings**, click **New variable**.
8. Set **Key** to `TOKEN`. Set **Value** to your group token from Deschtimes.
9. **Deploy site**.

### Deploying on your own server

```sh
# Build the server
npm run build
# Start the server at a custom port, e.g. 8080
npx cross-env TOKEN=INSERT_TOKEN_HERE svelte-kit start -p 8080
```

Point your webserver at the application. The method will vary based on what you're running (e.g. Apache, nginx) and is out of the scope of this README.

## Wordpress HTML Widget

After deploying to one of the options above, adjust the following script.

Replace both instances of `http://localhost:4100` with your application (e.g. `https://blah-blah-1234.netlify.com`)

```html
<script>
    /**
     * Resize the iframe after page load
     */
    window.addEventListener(
        'message',
        function (event) {
            if (event.origin !== 'http://localhost:4100') {
                return;
            }

            const data = JSON.parse(event.data);
            const iframe = document.getElementById('areWeThereYet');
            if (data.action === 'resize') {
                iframe.height = data.height;
                iframe.style.opacity = '1';
            }
        },
        false
    );
</script>
<iframe
    id="areWeThereYet"
    src="http://localhost:4100"
    allowtransparency="true"
    frameborder="0"
    style="opacity: 0; min-width: 100%;"
></iframe>
```

# License

Dual-licensed under either MIT license or Apache License (Version 2.0), under your convenience.
