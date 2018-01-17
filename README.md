## dockselpy

Dockerfile example on how to *"assemble"* together Selenium (with support for Chrome, Firefox and PhantomJS), Python and Xfvb.

### Information

Recent struggle with finding a docker image for Selenium that supports headless versions for both Firefox and Chrome, 
led to the process of building my own version.

The image is build with the following dependencies:
- latest Chrome and chromedriver
- latest Firefox and geckodriver
- latest stable PhantomJS webkit (v2.1.1)
- Selenium
- Python 3
- Xvfb and the python wrapper - pyvirtualdisplay


### Running selenium in the server machine:

- docker-compose

    ```
    docker-compose stop && docker-compose build && docker-compose up -d
    ```
    
### Example python connect remote

```python
# To install the Python client library:
# pip install -U selenium

from selenium import webdriver
from selenium.webdriver.common.desired_capabilities import DesiredCapabilities
import csv

# Firefox 
driver = webdriver.Remote(
   command_executor='http://<IP-MACHINE-SERVER>:4000/wd/hub',
   desired_capabilities=DesiredCapabilities.CHROME)

# ------------------------------
# Go to codepad.org
driver.get('http://codepad.org')

# Select the Python language option
python_link = driver.find_elements_by_xpath("//input[@name='lang' and @value='Python']")[0]
python_link.click()

# Enter some text!
text_area = driver.find_element_by_id('textarea')
text_area.send_keys("print 'Hello,' + ' World!'")

# Submit the form!
submit_button = driver.find_element_by_name('submit')
submit_button.click()

# Make this an actual test. Isn't Python beautiful?
assert "Hello, World!" in driver.get_page_source()

#save csv
with open('c:/dataset.csv', 'wb') as csvfile: #save in your local machine exection
    spamwriter = csv.writer(csvfile, delimiter=' ',
                            quotechar='|', quoting=csv.QUOTE_MINIMAL)
    spamwriter.writerow(['Spam'] * 5 + ['Baked Beans'])
    spamwriter.writerow(['Spam', 'Lovely Spam', 'Wonderful Spam'])

# Close the browser!
driver.quit()
```
    
### Example python inside the server

```python
from pyvirtualdisplay import Display
from selenium import webdriver

display = Display(visible=0, size=(800, 600))
display.start()

browser = webdriver.Firefox()
browser.get('https://www.google.com/')
print(browser.title)

browser.quit()
display.stop()

```

Detailed examples on how to use Firefox with custom profile, Google Chrome with desired options or PhantomJS can be found in the source.
