Fritzremote
============
Let´s you **remote control** parts of your **FRITZ!Box** via selenium.

Fritzremote is collection of predefined functions which gives you the ability to remote control your `http://fritz.box` 
userinterface. Those of you who are familiar with the Fritz!Box ecosystem might think: "But there is an official API 
from AVM". And yes, that's what I also thought. But there are parts that are not covered. 

Fritzremote tries to fill this gaps.

Selenium
-------------
Fritzremote basically is just a wrapper around a python package called: [Selenium](https://pypi.org/project/selenium/).
So to remote control your Fritz!Box, you need to pass a selenium webdriver object to Fritzremote. See examples below.

For further information on how to configure, install a executable driver and so on, please visit the official 
[Documentation](https://selenium-python.readthedocs.io/) and/or find yourself some nice tutorials.
There are plenty of them.

Selenium comes in a local and a remote version. 

#### Remote Version
If you want to run the browser on an other device than the one your
python application that uses Fritzremote, you need to install a little java based server. 

#### Local Version
For the local version, Fritzremote needs a browser specific driver executable. You can can download them
here: https://seleniumhq.github.io/selenium/docs/api/py/#drivers. Several browsers/drivers are supported 
(Firefox, Chrome, Internet Explorer), as well as the Remote protocol.

More on both in the official [Documentation](https://selenium-python.readthedocs.io/).

#### Example 1 (Remote Device)
    
    options = webdriver.ChromeOptions()
    options.headless = True
    options.add_argument('--window-size=1920x1080')

For performance reasons we activate the daemon mode by setting headless = True (Set it to False for debugging).
Next comes an important part: `options.add_argument('--window-size=1920x1080')`. If you don't, Fritzremote won't find
the elements and will not work as expected, even though we set `headless = True`.

Next we instantiate the driver object with the following arguments:

    driver = webdriver.Remote(
        command_executor=f'http://XXX.XXX.XXX.XX:4444/wd/hub',
        desired_capabilities=DesiredCapabilities.CHROME,
        options=options
    )

**command_executor** : `str`

The address of the device where the selenium server is running at.

**desired_capabilities**: `Dict[str, str]`
 
Here we need an DesiredCapabilities object and get the browser specific settings accessing the corresponding property.
In our example we will use chrome. So the CHROME property actually is just a `dictionary` with the following values:

    CHROME = {
        "browserName": "chrome",
        "version": "",
        "platform": "ANY",
    }
    
Last but not least we pass in the `options` variable with the `chromeOptions()` object.


#### Example 2 (Local Device)

There is not much what we have to change to run selenium locally. First set configurations.

    options = webdriver.ChromeOptions()
    options.headless = daemon_mode
    options.add_argument('--window-size=1920x1080')
    
Then get the browser specific driver from the webDriver module. In my case I used chrome. This object inherits
already the values which we pased to the `desired_capabilities` argument in **Example 1**.

    driver = webdriver.Chrome(executable_path='/usr/local/bin/chromedriver', options=options)
    
But in order to work properly we need to tell selenium where to find the browser driver executable. If it's not set,
the default is used and it assumes the executable is in the $PATH.

Wifi-Control
-------------
