---
layout: post
title: Nailed it with Defang
author: Lionello Lunesu
date: 2024-11-18 17:34:00.000000000 -08:00
---
{:refdef: style="text-align: center;"}
![The Greek god Hephaestus striking a USB thumb drive with his hammer in the Cloud](/images/1418fc22c430809baab5f1165ed297db/image.png){: width="350" }
{: refdef}

Christmas is nearing, and this year I’m once again spending time in the Vancouver Hacker Space [vanhack.ca](http://vanhack.ca) to work on a silly Christmas project to turn a human-sized* acrylic sphere into a snow globe. I haven’t finished it in the last few years, and I likely won’t finish it this winter either, but it’s still a fun project to tinker on.

The project was inspired by [Knick Knack](https://en.wikipedia.org/wiki/Knick_Knack), one of Pixar’s earliest 3D shorts. My plan was to fill the globe with a 2D cut-out of Knick Knack the Snowman and his igloo, along with fake snow. Then, I'd add an Arduino and an accelerometer to detect a “shake” and in turn trigger the snow.

The most challenging part of this project is figuring out how to make the fake snow particles flow around the sphere.  Making the snow fly around nicely involves the strategic placement of a fan. On this particular day, I figured I’d use the laser cutter to cut holes out of a wooden disc that would become the floor of the globe and snuggly fit the back of a fan that I had found lying around.

{:refdef: style="text-align: center;"}
![Lio struggling to fetch the snow globe](/images/1418fc22c430809baab5f1165ed297db/IMG_1673_lio_globe.jpg){: width="250" }
{: refdef}

I didn’t bring my laptop, so I used [Inkscape](https://inkscape.org/) on one of the publicly available computers to draw the outline of the disc as well as the circle shaped hole in the middle that would fit the back of the fan (and its power cable).

Having finished the drawing and exported it to a file, I now had to find a way to transfer the file to the computer with the laser cutter software. The hackerspace has Wi-Fi but I did not feel like digging into networking or dealing with account and file permissions on these public Windows machines. I looked around the space for USB thumb drives, but didn’t find any. I did bring my phone, and perhaps could find a lightning cable, but I also wasn’t particularly eager to connect my phone to this public PC. If I could just upload my file somewhere, then I could easily download it on my phone and, later, on the laser cutter’s PC.

> [“Give a boy a hammer and everything he meets has to be pounded.” - Abraham Kaplan](https://en.wikipedia.org/wiki/Law_of_the_instrument) (not Mark Twain)
>

I've spent the last year building [Defang](https://defang.io). `defang` is a tool which deploys Docker compose-based projects to the cloud using `defang compose up`. To lower the barrier to adoption, we added a gen-AI command (`defang generate`) to help you get started with a project outline. We also provide a [Defang Playground](https://docs.defang.io/docs/concepts/defang-playground) to temporarily deploy your project before you’re ready to [bring-your-own-cloud](https://docs.defang.io/docs/concepts/defang-byoc). Fully aware I was now “a boy with a hammer” staring at a file-shaped nail, I realized I could build an app without needing to install any other dependencies like Docker/Python/NodeJS on this PC, since Defang would build the app in the cloud for me. Being in a [hackerspace](https://hackerspaces.org), it’s quite fitting to be using tools and materials in ways they were not initially intended to be used.

As I was on a Windows machine, my first try was to use the newish `winget` command on the command prompt:

```
C:\Users\Administrator> winget install defang
Found Defang [DefangLabs.Defang] Version 0.6.4
This application is licensed to you by its owner.
Microsoft is not responsible for, nor does it grant any licenses to, third-party packages.
Downloading [https://s.defang.io/defang_0.6.4_windows_amd64.zip?x-defang-source=winget](https://s.defang.io/defang_0.6.4_windows_amd64.zip?x-defang-source=winget)
  ██████████████████████████████  11.0 MB / 11.0 MB
Successfully verified installer hash
Extracting archive...
Successfully extracted archive
Starting package install...
**Command line alias added: "defang"
Path environment variable modified; restart your shell to use the new value.**
Successfully installed
```

That worked like a charm!

I then used `defang generate` to generate all the code I need for a website to upload/download my file:

```
C:\Users\Administrator> defang generate
 * Using Defang Playground; consider using BYOC ([https://s.defang.io/byoc](https://s.defang.io/byoc))
**? Choose a sample service:** *Generate with AI*
**? Choose the language you'd like to use:** *Nodejs*
**? Please describe the service you'd like to build:** *app that on / shows an HTML with file upload form and and list of previously uploaded files you can click on to download*
**? What folder would you like to create the project in?** *project1*
Our latest terms of service can be found at [https://defang.io/terms-service.html](https://defang.io/terms-service.html)
**? Do you agree to the Defang terms of service?** *Yes*
 * Working on it. This may take 1 or 2 minutes...
 * Writing files to disk...
   - Dockerfile
   - compose.yaml
   - main.js
   - package.json
 * Code generated successfully in folder project1

Check the files in your favorite editor.
To deploy the service, do `cd project1` and

  defang compose up
```

OK, so `defang` generated [these 4 files](https://gist.github.com/lionello/d1f9d5c198f12590fb70997a93f06edd):

<script>
function toggleVisibility(id) {
    var element = document.getElementById(id);
    if (element.style.display === "none") {
        element.style.display = "block";
    } else {
        element.style.display = "none";
    }
}
</script>

- `Dockerfile` was pretty standard for a NodeJS app, based off of Ubuntu-slim
    <button onclick="toggleVisibility('content1')">Show/Hide</button>
    <div id="content1" style="display:none">{% gist d1f9d5c198f12590fb70997a93f06edd Dockerfile %}</div>

- `compose.yaml` again, pretty standard with only a single service, built from source in `.` and exposing port 3000
    <button onclick="toggleVisibility('content2')">Show/Hide</button>
    <div id="content2" style="display:none">{% gist d1f9d5c198f12590fb70997a93f06edd compose.yaml %}</div>

- `main.js` ExpressJS listening on port 3000, with `GET` on `/` returning an HTML form, like I had asked
    <button onclick="toggleVisibility('content3')">Show/Hide</button>
    <div id="content3" style="display:none">{% gist d1f9d5c198f12590fb70997a93f06edd main.js %}</div>

- `package.json` with `express` and `multer` dependencies
    <button onclick="toggleVisibility('content4')">Show/Hide</button>
    <div id="content4" style="display:none">{% gist d1f9d5c198f12590fb70997a93f06edd package.json %}</div>

On a quick inspection all files looked great so I was ready to deploy to the Playground with `defang compose up`:

```
C:\Users\Administrator> cd project1

C:\Users\Administrator\project1> defang compose up
 * Using Defang Playground; consider using BYOC ([https://s.defang.io/byoc](https://s.defang.io/byoc))
 ! Please log in to continue.
Please visit http://127.0.0.1:59734 and log in. (Right click the URL or press ENTER to open browser)
```

This was the first time I was prompted to log in. Defang relies on a GitHub account for login. I was on a public computer and didn’t want to log in with my own GitHub account, so I quickly signed up for a new account during the login process and the deployment proceeded:

```
 * Successfully logged in to fabric-prod1.defang.dev:443 (default tenant)
 * Using Defang Playground; consider using BYOC ([https://s.defang.io/byoc](https://s.defang.io/byoc))
 ! service "service1": missing memory reservation; using provider-specific defaults. Specify deploy.resources.reservations.memory to avoid out-of-memory errors
 * Packaging the project files for service1 at /Users/llunesu/dev/defang/project1
 * Uploading the project files for service1
 * Monitor your services' status in the defang portal
   - https://portal.defang.dev/service/service1
 * Tailing logs for deployment ID j4lw4g9gofik ; press Ctrl+C to detach:
 * Showing only build logs and runtime errors. Press V to toggle verbose mode.
2024-11-02T15:13:59.616-07:00 cd Update started for stack lionello-prod1
2024-11-02T15:14:04.037-07:00 cd  ** Building image for "service1"...
…
2024-11-02T15:15:14.832-07:00 cd  ** Build succeeded for "service1"
2024-11-02T15:15:15.410-07:00 cd  ** Updated service "service1" to revision 147
2024-11-02T15:15:24.192-07:00 cd Update succeeded in 1m24.609245342s ; provisioning...
2024-11-02T15:15:58.246-07:00 service1 Server started on port 3000
 * Service service1 is in state DEPLOYMENT_COMPLETED and will be available at:
   - https://lionello-service1--3000.prod1a.defang.dev
 * Done.
For help with warnings, check our FAQ at [https://docs.defang.io/docs/faq](https://docs.defang.io/docs/faq)
```

After 2 minutes** Defang reserved a URL for my service and printed it to my shell. When I opened the link, I saw the HTML upload form I expected. I pressed “Choose File”, selected the SVG file I had exported form Inkscape, and uploaded it:

{:refdef: style="text-align: center;"}
![image.png](/images/1418fc22c430809baab5f1165ed297db/image%201.png){: width="250" }
{: refdef}

If my experience building [Nounly](https://noun.ly) taught me anything, it was that it would only be a matter of seconds before bots started submitting spam and other junk to open HTML forms. So, I decided to deprovision the site immediately using `defang compose down`.

By no means do I claim that this was a sensible way to get a file off a computer. Considering I was using a GitHub account for login, I might as well have created a [gist](https://gist.github.com). Even so, it was very satisfying to be able to use a tool I had built to solve a problem it wasn’t particularly built for.

If you give Defang a try, let me know what you think! The CLI currently supports AWS, but there’s limited support for DigitalOcean and we’re working to have GCP hooked up in a few weeks.

* I had asked ChatGPT 4o whether “human-sized” was a good descriptor for the size of the globe and it turned into horror fairly quickly:

![image.png](/images/1418fc22c430809baab5f1165ed297db/image%202.png)

```python
import math

# Constants
radius_m = 1 / 2  # radius of the sphere in meters (1m diameter)
volume_sphere = (4/3) * math.pi * radius_m**3  # volume of the sphere in cubic meters

# Average human volume in cubic meters (assume average adult)
average_human_volume = 0.07  # typical range is ~0.065 to 0.075 m^3

# Calculate how many humans fit
humans_fit = volume_sphere / average_human_volume
humans_fit
```

> [“We've got room for a-whole-nother two-thirds of a person.” - Bender (Futurama S1E3)](https://theinfosphere.org/Transcript:I,_Roommate#time-07-51)
>

** the first deployment will always be slower, because none of the container layers are cached. `defang` will also wait for a few healthchecks to pass before it prints the final URL.