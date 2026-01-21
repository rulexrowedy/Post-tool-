import streamlit as st
import time
import threading
import os
from pathlib import Path
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.chrome.options import Options
import database as db

st.set_page_config(
    page_title="FB Comment Tool",
    page_icon="💬",
    layout="wide",
    initial_sidebar_state="expanded"
)

custom_css = """
<style>
    @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap');
    
    * {
        font-family: 'Poppins', sans-serif;
    }
    
    .stApp {
        background-image: url('https://i.postimg.cc/TYhXd0gG/d0a72a8cea5ae4978b21e04a74f0b0ee.jpg');
        background-size: cover;
        background-position: center;
        background-repeat: no-repeat;
        background-attachment: fixed;
    }
    
    .main .block-container {
        background: rgba(255, 255, 255, 0.08);
        backdrop-filter: blur(8px);
        border-radius: 12px;
        padding: 25px;
        box-shadow: 0 2px 12px rgba(0, 0, 0, 0.15);
        border: 1px solid rgba(255, 255, 255, 0.12);
    }
    
    .main-header {
        background: rgba(255, 255, 255, 0.1);
        backdrop-filter: blur(10px);
        padding: 2rem;
        border-radius: 15px;
        text-align: center;
        margin-bottom: 2rem;
        box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
        border: 1px solid rgba(255, 255, 255, 0.15);
    }
    
    .main-header h1 {
        background: linear-gradient(45deg, #ff6b6b, #4ecdc4);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
        font-size: 2.5rem;
        font-weight: 700;
        margin: 0;
    }
    
    .main-header p {
        color: rgba(255, 255, 255, 0.9);
        font-size: 1.1rem;
        margin-top: 0.5rem;
    }
    
    .stButton>button {
        background: linear-gradient(45deg, #ff6b6b, #4ecdc4);
        color: white;
        border: none;
        border-radius: 10px;
        padding: 0.75rem 2rem;
        font-weight: 600;
        font-size: 1rem;
        transition: all 0.3s ease;
        box-shadow: 0 4px 15px rgba(102, 126, 234, 0.4);
        width: 100%;
    }
    
    .stButton>button:hover {
        opacity: 0.9;
        transform: translateY(-2px);
        box-shadow: 0 6px 20px rgba(102, 126, 234, 0.6);
    }
    
    .stTextInput>div>div>input, 
    .stTextArea>div>div>textarea, 
    .stNumberInput>div>div>input {
        background: rgba(255, 255, 255, 0.15);
        border: 1px solid rgba(255, 255, 255, 0.25);
        border-radius: 8px;
        color: white;
        padding: 0.75rem;
        transition: all 0.3s ease;
    }
    
    .stTextInput>div>div>input::placeholder,
    .stTextArea>div>div>textarea::placeholder {
        color: rgba(255, 255, 255, 0.6);
    }
    
    .stTextInput>div>div>input:focus, 
    .stTextArea>div>div>textarea:focus {
        background: rgba(255, 255, 255, 0.2);
        border-color: #4ecdc4;
        box-shadow: 0 0 0 2px rgba(78, 205, 196, 0.2);
        color: white;
    }
    
    label {
        color: white !important;
        font-weight: 500 !important;
        font-size: 14px !important;
    }
    
    .stTabs [data-baseweb="tab-list"] {
        gap: 8px;
        background: rgba(255, 255, 255, 0.06);
        padding: 10px;
        border-radius: 10px;
    }
    
    .stTabs [data-baseweb="tab"] {
        background: rgba(255, 255, 255, 0.1);
        border-radius: 8px;
        color: white;
        padding: 10px 20px;
    }
    
    .stTabs [aria-selected="true"] {
        background: linear-gradient(45deg, #ff6b6b, #4ecdc4);
    }
    
    [data-testid="stMetricValue"] {
        color: #4ecdc4;
        font-weight: 700;
        font-size: 1.8rem;
    }
    
    [data-testid="stMetricLabel"] {
        color: rgba(255, 255, 255, 0.9);
        font-weight: 500;
    }
    
    .console-output {
        background: rgba(0, 0, 0, 0.5);
        border: 1px solid rgba(78, 205, 196, 0.4);
        border-radius: 10px;
        padding: 12px;
        font-family: 'Courier New', 'Consolas', 'Monaco', monospace;
        font-size: 12px;
        color: #00ff88;
        line-height: 1.6;
        max-height: 400px;
        overflow-y: auto;
    }
    
    .console-line {
        margin-bottom: 3px;
        word-wrap: break-word;
        padding: 6px 10px;
        padding-left: 28px;
        color: #00ff88;
        background: rgba(78, 205, 196, 0.08);
        border-left: 2px solid rgba(78, 205, 196, 0.4);
        position: relative;
    }
    
    .console-line::before {
        content: '►';
        position: absolute;
        left: 10px;
        opacity: 0.6;
        color: #4ecdc4;
    }
    
    .success-box {
        background: linear-gradient(135deg, #84fab0 0%, #8fd3f4 100%);
        padding: 1rem;
        border-radius: 10px;
        color: white;
        text-align: center;
        margin: 1rem 0;
    }
    
    .error-box {
        background: linear-gradient(135deg, #fa709a 0%, #fee140 100%);
        padding: 1rem;
        border-radius: 10px;
        color: white;
        text-align: center;
        margin: 1rem 0;
    }
    
    .footer {
        text-align: center;
        padding: 2rem;
        color: rgba(255, 255, 255, 0.7);
        font-weight: 600;
        margin-top: 3rem;
        background: rgba(255, 255, 255, 0.05);
        border-radius: 10px;
    }
    
    [data-testid="stSidebar"] {
        background: rgba(0, 0, 0, 0.3);
        backdrop-filter: blur(10px);
    }
    
    [data-testid="stSidebar"] .element-container {
        color: white;
    }
</style>
"""

st.markdown(custom_css, unsafe_allow_html=True)

if 'logged_in' not in st.session_state:
    st.session_state.logged_in = False
if 'user_id' not in st.session_state:
    st.session_state.user_id = None
if 'username' not in st.session_state:
    st.session_state.username = None
if 'logs' not in st.session_state:
    st.session_state.logs = []

class AutomationState:
    def __init__(self, session_id=1):
        self.session_id = session_id
        self.running = False
        self.comment_count = 0
        self.logs = []
        self.comment_rotation_index = 0
        self.post_id = ''
        self.comment_prefix = ''
        self.delay = 30
        self.cookies = ''
        self.comments = ''

if 'sessions' not in st.session_state:
    st.session_state.sessions = {}

if 'auto_start_checked' not in st.session_state:
    st.session_state.auto_start_checked = False

def get_or_create_session(session_id):
    if session_id not in st.session_state.sessions:
        st.session_state.sessions[session_id] = AutomationState(session_id)
    return st.session_state.sessions[session_id]

def log_message(msg, automation_state=None):
    timestamp = time.strftime("%H:%M:%S")
    formatted_msg = f"[{timestamp}] {msg}"
    
    if automation_state:
        automation_state.logs.append(formatted_msg)
    else:
        if 'logs' in st.session_state:
            st.session_state.logs.append(formatted_msg)

def setup_browser(automation_state=None):
    log_message('Setting up Chrome browser...', automation_state)
    
    chrome_options = Options()
    chrome_options.add_argument('--headless=new')
    chrome_options.add_argument('--no-sandbox')
    chrome_options.add_argument('--disable-setuid-sandbox')
    chrome_options.add_argument('--disable-dev-shm-usage')
    chrome_options.add_argument('--disable-gpu')
    chrome_options.add_argument('--disable-extensions')
    chrome_options.add_argument('--window-size=1920,1080')
    chrome_options.add_argument('--user-agent=Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/121.0.0.0 Safari/537.36')
    
    chromium_paths = [
        '/usr/bin/chromium',
        '/usr/bin/chromium-browser',
        '/usr/bin/google-chrome',
        '/usr/bin/chrome'
    ]
    
    for chromium_path in chromium_paths:
        if Path(chromium_path).exists():
            chrome_options.binary_location = chromium_path
            log_message(f'Found Chromium at: {chromium_path}', automation_state)
            break
    
    chromedriver_paths = [
        '/usr/bin/chromedriver',
        '/usr/local/bin/chromedriver'
    ]
    
    driver_path = None
    for driver_candidate in chromedriver_paths:
        if Path(driver_candidate).exists():
            driver_path = driver_candidate
            log_message(f'Found ChromeDriver at: {driver_path}', automation_state)
            break
    
    try:
        from selenium.webdriver.chrome.service import Service
        
        if driver_path:
            service = Service(executable_path=driver_path)
            driver = webdriver.Chrome(service=service, options=chrome_options)
            log_message('Chrome started with detected ChromeDriver!', automation_state)
        else:
            driver = webdriver.Chrome(options=chrome_options)
            log_message('Chrome started with default driver!', automation_state)
        
        driver.set_window_size(1920, 1080)
        log_message('Chrome browser setup completed successfully!', automation_state)
        return driver
    except Exception as error:
        log_message(f'Browser setup failed: {error}', automation_state)
        raise error

def find_comment_input(driver, process_id, automation_state=None):
    log_message(f'{process_id}: Finding comment input...', automation_state)
    time.sleep(10)
    
    try:
        driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
        time.sleep(2)
        driver.execute_script("window.scrollTo(0, 0);")
        time.sleep(2)
    except Exception:
        pass
    
    comment_input_selectors = [
        'div[contenteditable="true"][role="textbox"]',
        'div[contenteditable="true"][data-lexical-editor="true"]',
        'div[aria-label*="comment" i][contenteditable="true"]',
        'div[aria-label*="Comment" i][contenteditable="true"]',
        'div[aria-label*="Write a comment" i][contenteditable="true"]',
        'div[contenteditable="true"][spellcheck="true"]',
        '[role="textbox"][contenteditable="true"]',
        'textarea[placeholder*="comment" i]',
        'div[aria-placeholder*="comment" i]',
        'div[data-placeholder*="comment" i]',
        '[contenteditable="true"]',
        'textarea',
        'input[type="text"]'
    ]
    
    log_message(f'{process_id}: Trying {len(comment_input_selectors)} selectors...', automation_state)
    
    for idx, selector in enumerate(comment_input_selectors):
        try:
            elements = driver.find_elements(By.CSS_SELECTOR, selector)
            log_message(f'{process_id}: Selector {idx+1}/{len(comment_input_selectors)} found {len(elements)} elements', automation_state)
            
            for element in elements:
                try:
                    is_editable = driver.execute_script("""
                        return arguments[0].contentEditable === 'true' || 
                               arguments[0].tagName === 'TEXTAREA' || 
                               arguments[0].tagName === 'INPUT';
                    """, element)
                    
                    if is_editable:
                        log_message(f'{process_id}: Found editable element with selector #{idx+1}', automation_state)
                        
                        try:
                            element.click()
                            time.sleep(0.5)
                        except:
                            pass
                        
                        element_text = driver.execute_script("return arguments[0].placeholder || arguments[0].getAttribute('aria-label') || arguments[0].getAttribute('aria-placeholder') || '';", element).lower()
                        
                        keywords = ['comment', 'write', 'type', 'reply', 'text']
                        if any(keyword in element_text for keyword in keywords):
                            log_message(f'{process_id}: Found comment input with text: {element_text[:50]}', automation_state)
                            return element
                        elif idx < 10:
                            log_message(f'{process_id}: Using primary selector editable element (#{idx+1})', automation_state)
                            return element
                except Exception as e:
                    continue
        except Exception as e:
            continue
    
    return None

def get_next_comment(comments, automation_state=None):
    if not comments or len(comments) == 0:
        return 'Nice post!'
    
    if automation_state:
        comment = comments[automation_state.comment_rotation_index % len(comments)]
        automation_state.comment_rotation_index += 1
    else:
        comment = comments[0]
    
    return comment

def post_comments(config, automation_state, user_id, process_id='AUTO-1'):
    driver = None
    try:
        log_message(f'{process_id}: Starting comment automation...', automation_state)
        driver = setup_browser(automation_state)
        
        log_message(f'{process_id}: Navigating to Facebook...', automation_state)
        driver.get('https://www.facebook.com/')
        time.sleep(8)
        
        if config['cookies'] and config['cookies'].strip():
            log_message(f'{process_id}: Adding cookies...', automation_state)
            cookie_array = config['cookies'].split(';')
            for cookie in cookie_array:
                cookie_trimmed = cookie.strip()
                if cookie_trimmed:
                    first_equal_index = cookie_trimmed.find('=')
                    if first_equal_index > 0:
                        name = cookie_trimmed[:first_equal_index].strip()
                        value = cookie_trimmed[first_equal_index + 1:].strip()
                        try:
                            driver.add_cookie({
                                'name': name,
                                'value': value,
                                'domain': '.facebook.com',
                                'path': '/'
                            })
                        except Exception:
                            pass
        
        if config['post_id']:
            post_id = config['post_id'].strip()
            log_message(f'{process_id}: Opening post {post_id}...', automation_state)
            if post_id.startswith('http://') or post_id.startswith('https://'):
                driver.get(post_id)
            else:
                driver.get(f'https://www.facebook.com/{post_id}')
        else:
            log_message(f'{process_id}: No post ID provided!', automation_state)
            automation_state.running = False
            db.set_automation_running(user_id, False)
            return 0
        
        time.sleep(15)
        
        log_message(f'{process_id}: ID Online - Browser Active', automation_state)
        
        delay = int(config['delay'])
        comments_sent = 0
        comments_list = [c.strip() for c in config['comments'].split('\n') if c.strip()]
        
        if not comments_list:
            comments_list = ['Nice post!']
        
        while automation_state.running:
            comment_input = find_comment_input(driver, process_id, automation_state)
            
            if not comment_input:
                log_message(f'{process_id}: Comment input not found, refreshing page...', automation_state)
                driver.refresh()
                time.sleep(10)
                continue
            
            base_comment = get_next_comment(comments_list, automation_state)
            
            if config['comment_prefix']:
                comment_to_send = f"{config['comment_prefix']} {base_comment}"
            else:
                comment_to_send = base_comment
            
            try:
                from selenium.webdriver.common.action_chains import ActionChains
                
                driver.execute_script("""
                    const element = arguments[0];
                    element.scrollIntoView({behavior: 'smooth', block: 'center'});
                """, comment_input)
                time.sleep(0.5)
                
                driver.execute_script("arguments[0].click();", comment_input)
                time.sleep(0.3)
                
                driver.execute_script("""
                    const element = arguments[0];
                    element.focus();
                    if (element.tagName === 'DIV') {
                        element.innerHTML = '';
                        element.textContent = '';
                    } else {
                        element.value = '';
                    }
                """, comment_input)
                time.sleep(0.2)
                
                actions = ActionChains(driver)
                actions.move_to_element(comment_input)
                actions.click()
                actions.send_keys(comment_to_send)
                actions.perform()
                time.sleep(0.5)
                
                log_message(f'{process_id}: Comment typed, sending Enter...', automation_state)
                
                comment_input.send_keys(Keys.ENTER)
                time.sleep(1)
                
                comments_sent += 1
                automation_state.comment_count = comments_sent
                log_message(f'{process_id}: Comment #{comments_sent} posted: {comment_to_send[:50]}...', automation_state)
                
                log_message(f'{process_id}: Waiting {delay} seconds before next comment...', automation_state)
                
                for i in range(delay):
                    if not automation_state.running:
                        break
                    time.sleep(1)
                
            except Exception as e:
                log_message(f'{process_id}: Error posting comment: {str(e)[:100]}', automation_state)
                time.sleep(5)
        
        log_message(f'{process_id}: Automation stopped. Total comments: {comments_sent}', automation_state)
        return comments_sent
        
    except Exception as error:
        log_message(f'{process_id}: Fatal error: {str(error)[:200]}', automation_state)
        return 0
    finally:
        automation_state.running = False
        db.set_automation_running(user_id, False)
        if driver:
            try:
                driver.quit()
            except:
                pass

def start_automation_thread(config, automation_state, user_id):
    automation_state.running = True
    automation_state.logs = []
    automation_state.comment_count = 0
    automation_state.comment_rotation_index = 0
    
    db.set_automation_running(user_id, True)
    
    thread = threading.Thread(
        target=post_comments,
        args=(config, automation_state, user_id),
        daemon=True
    )
    thread.start()

def stop_automation(automation_state, user_id):
    automation_state.running = False
    db.set_automation_running(user_id, False)

def show_login_page():
    st.markdown("""
        <div class="main-header">
            <h1>FB Comment Tool</h1>
            <p>Facebook Post Comment Automation</p>
        </div>
    """, unsafe_allow_html=True)
    
    tab1, tab2 = st.tabs(["Login", "Register"])
    
    with tab1:
        st.markdown("### Login to Your Account")
        login_username = st.text_input("Username", key="login_username", placeholder="Enter your username")
        login_password = st.text_input("Password", type="password", key="login_password", placeholder="Enter your password")
        
        if st.button("Login", key="login_btn"):
            if login_username and login_password:
                user_id = db.verify_user(login_username, login_password)
                if user_id:
                    st.session_state.logged_in = True
                    st.session_state.user_id = user_id
                    st.session_state.username = login_username
                    st.success("Login successful!")
                    st.rerun()
                else:
                    st.error("Invalid username or password!")
            else:
                st.warning("Please enter both username and password!")
    
    with tab2:
        st.markdown("### Create New Account")
        reg_username = st.text_input("Username", key="reg_username", placeholder="Choose a username")
        reg_password = st.text_input("Password", type="password", key="reg_password", placeholder="Choose a password")
        reg_confirm = st.text_input("Confirm Password", type="password", key="reg_confirm", placeholder="Confirm your password")
        
        if st.button("Register", key="register_btn"):
            if reg_username and reg_password and reg_confirm:
                if reg_password == reg_confirm:
                    if len(reg_password) >= 6:
                        success, message = db.create_user(reg_username, reg_password)
                        if success:
                            st.success(message)
                        else:
                            st.error(message)
                    else:
                        st.warning("Password must be at least 6 characters!")
                else:
                    st.error("Passwords do not match!")
            else:
                st.warning("Please fill all fields!")

def show_main_app():
    st.markdown("""
        <div class="main-header">
            <h1>FB Comment Tool</h1>
            <p>Facebook Post Comment Automation - Multi Session</p>
        </div>
    """, unsafe_allow_html=True)
    
    with st.sidebar:
        st.markdown(f"### Welcome, {st.session_state.username}!")
        
        if st.button("Logout"):
            st.session_state.logged_in = False
            st.session_state.user_id = None
            st.session_state.username = None
            st.rerun()
        
        st.markdown("---")
        st.markdown("### Active Sessions")
        
        total_comments = 0
        running_count = 0
        for sid, session in st.session_state.sessions.items():
            if session.running:
                running_count += 1
                st.markdown(f'<div class="success-box">Session {sid}: Running ({session.comment_count} comments)</div>', unsafe_allow_html=True)
            total_comments += session.comment_count
        
        if running_count == 0:
            st.markdown('<div class="error-box">No Active Sessions</div>', unsafe_allow_html=True)
        
        st.metric("Total Comments", total_comments)
        st.metric("Active Sessions", running_count)
    
    if 'current_session' not in st.session_state:
        st.session_state.current_session = 1
    
    col_add, col_refresh = st.columns(2)
    with col_add:
        if st.button("+ Add New Session", use_container_width=True):
            new_id = max(st.session_state.sessions.keys(), default=0) + 1
            get_or_create_session(new_id)
            st.session_state.current_session = new_id
            st.rerun()
    with col_refresh:
        if st.button("Refresh All", use_container_width=True):
            st.rerun()
    
    if not st.session_state.sessions:
        get_or_create_session(1)
    
    session_tabs = st.tabs([f"Session {sid}" for sid in sorted(st.session_state.sessions.keys())])
    
    for idx, (sid, session) in enumerate(sorted(st.session_state.sessions.items())):
        with session_tabs[idx]:
            render_session_ui(sid, session)
    
    st.markdown("""
        <div class="footer">
            FB Comment Tool - Multi Session Support
        </div>
    """, unsafe_allow_html=True)

def render_session_ui(session_id, automation_state):
    config = db.get_user_config(st.session_state.user_id)
    
    status_col, stop_col = st.columns([3, 1])
    with status_col:
        if automation_state.running:
            st.success(f"Session {session_id} is RUNNING - {automation_state.comment_count} comments posted")
        else:
            st.info(f"Session {session_id} is STOPPED")
    with stop_col:
        if automation_state.running:
            if st.button("Stop", key=f"stop_{session_id}", use_container_width=True):
                automation_state.running = False
                st.rerun()
    
    col1, col2 = st.columns(2)
    
    with col1:
        post_id = st.text_input(
            "Post ID / URL",
            value=automation_state.post_id or (config['post_id'] if config else ''),
            placeholder="https://www.facebook.com/page/posts/123...",
            key=f"post_id_{session_id}",
            help="Enter full Facebook post URL"
        )
        
        comment_prefix = st.text_input(
            "Comment Prefix (Optional)",
            value=automation_state.comment_prefix or (config['comment_prefix'] if config else ''),
            placeholder="e.g., @username",
            key=f"prefix_{session_id}"
        )
        
        delay = st.number_input(
            "Delay (seconds)",
            min_value=10,
            max_value=3600,
            value=automation_state.delay or (config['delay'] if config else 30),
            key=f"delay_{session_id}"
        )
    
    with col2:
        cookies = st.text_area(
            "Facebook Cookies",
            value=automation_state.cookies or (config['cookies'] if config else ''),
            height=120,
            placeholder="Paste your Facebook cookies here...",
            key=f"cookies_{session_id}"
        )
    
    uploaded_file = st.file_uploader(
        "Upload Comments (TXT)",
        type=['txt'],
        key=f"file_{session_id}"
    )
    
    if uploaded_file is not None:
        comments = uploaded_file.read().decode('utf-8')
    else:
        comments = st.text_area(
            "Comments (one per line)",
            value=automation_state.comments or (config['comments'] if config else ''),
            height=100,
            placeholder="Nice post!\nGreat content!",
            key=f"comments_{session_id}"
        )
    
    if not automation_state.running:
        if st.button("Start This Session", key=f"start_{session_id}", use_container_width=True):
            if not cookies:
                st.error("Please add cookies!")
            elif not post_id:
                st.error("Please enter Post ID!")
            elif not comments:
                st.error("Please add comments!")
            else:
                automation_state.post_id = post_id
                automation_state.comment_prefix = comment_prefix
                automation_state.delay = delay
                automation_state.cookies = cookies
                automation_state.comments = comments
                
                session_config = {
                    'post_id': post_id,
                    'comment_prefix': comment_prefix,
                    'delay': delay,
                    'cookies': cookies,
                    'comments': comments
                }
                
                start_automation_thread(session_config, automation_state, st.session_state.user_id)
                st.success(f"Session {session_id} started!")
                st.rerun()
    
    st.markdown("#### Console")
    if automation_state.logs:
        logs_html = '<div class="console-output">'
        for log in automation_state.logs[-50:]:
            logs_html += f'<div class="console-line">{log}</div>'
        logs_html += '</div>'
        st.markdown(logs_html, unsafe_allow_html=True)
    else:
        st.info("No logs yet.")

if st.session_state.logged_in:
    show_main_app()
else:
    show_login_page()
