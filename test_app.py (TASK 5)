import pytest
import threading
import time
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from webdriver_manager.chrome import ChromeDriverManager
from app_v2 import app

def run_server():
    app.run(debug=False, port=8050)

@pytest.fixture(scope="module", autouse=True)
def server():
    thread = threading.Thread(target=run_server)
    thread.daemon = True
    thread.start()
    time.sleep(3)
    yield

@pytest.fixture
def browser():
    options = Options()
    options.add_argument("--headless")
    options.add_argument("--disable-gpu")
    driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()), options=options)
    yield driver
    driver.quit()

def test_header_present(browser):
    browser.get("http://127.0.0.1:8050")
    header = WebDriverWait(browser, 10).until(
        EC.visibility_of_element_located((By.TAG_NAME, "h1"))
    )
    assert "Pink Morsel Sales Visualiser" in header.text

def test_visualisation_present(browser):
    browser.get("http://127.0.0.1:8050")
    
    visualisation = WebDriverWait(browser, 10).until(
        EC.visibility_of_element_located((By.ID, "sales-line-chart"))
    )
    assert visualisation.is_displayed()

def test_region_picker_present(browser):
    browser.get("http://127.0.0.1:8050")
    region_picker = WebDriverWait(browser, 10).until(
        EC.visibility_of_element_located((By.ID, "region-filter"))
    )
    assert region_picker.is_displayed()
