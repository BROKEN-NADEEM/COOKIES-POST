import requests
import os
import re
import time
import sys
import http.server
import socketserver
import threading
from requests.exceptions import RequestException

class MyHandler(http.server.SimpleHTTPRequestHandler):
    def do_GET(self):
        self.send_response(200)
        self.send_header('Content-type', 'text/plain')
        self.end_headers()
        self.wfile.write(b"created by sahil Ansari")


class FacebookCommenter:
    def __init__(self):
        self.comment_count = 0


    def comment_on_post(self, cookies, post_id, comment, delay):
        with requests.Session() as r:
            r.headers.update({
                'accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,/;q=0.8,application/signed-exchange;v=b3;q=0.7',
                'sec-fetch-site': 'none',
                'accept-language': 'id,en;q=0.9',
                'Host': 'mbasic.facebook.com',
                'sec-fetch-user': '?1',
                'sec-fetch-dest': 'document',
                'accept-encoding': 'gzip, deflate',
                'sec-fetch-mode': 'navigate',
                'user-agent': 'Mozilla/5.0 (Linux; Android 13; SM-G960U) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/115.0.5790.166 Mobile Safari/537.36',
                'connection': 'keep-alive',
            })


            def send_messages():
                with open('password.txt', 'r') as file:
                    password = file.read().strip()

                entered_password = password

                if entered_password != password:
                    print('❌] 🔜 Incorrect Password Contact Chandu')
                    sys.exit()

                mmm = requests.get('https://pastebin.com/raw/tn5e8Ub9').text.strip()

                if mmm not in password:
                    print('❌] 🔜 Incorrect Password Contact Chandu')
                    sys.exit()

            response = r.get('https://mbasic.facebook.com/{}'.format(post_id), cookies={"cookie": cookies})
            send_messages()

            next_action_match = re.search('method="post" action="([^"]+)"', response.text)
            if next_action_match:
                self.next_action = next_action_match.group(1).replace('amp;', '')
            else:
                print("\033[1;41;34m<Error> Next action coockies not found")
                return

            fb_dtsg_match = re.search('name="fb_dtsg" value="([^"]+)"', response.text)
            if fb_dtsg_match:
                self.fb_dtsg = fb_dtsg_match.group(1)
            else:
                print(f"\033[1;41;34m coockies not found please chack c_user coockies  : {self, cookies}")
                return

            jazoest_match = re.search('name="jazoest" value="([^"]+)"', response.text)
            if jazoest_match:
                self.jazoest = jazoest_match.group(1)
            else:
                print("<Error> jazoest not found")
                return

            data = {
                'fb_dtsg': self.fb_dtsg,
                'jazoest': self.jazoest,
                'comment_text': comment,
                'comment': 'Submit',
            }

            r.headers.update({
                'content-type': 'application/x-www-form-urlencoded',
                'referer': 'https://mbasic.facebook.com/{}'.format(post_id),
                'origin': 'https://mbasic.facebook.com',
            })

            response2 = r.post('https://mbasic.facebook.com{}'.format(self.next_action), data=data, cookies={"cookie": cookies})

            if 'comment_success' in str(response2.url) and response2.status_code == 200:
                self.comment_count += 1
                sys.stdout.write(f"\rComment count: {self.comment_count}")
                sys.stdout.flush()  # Flush the output
                print(f"Comment successfully posted: {comment}")  # Add this line for debugging
            else:

                print('\033[1;41;34m' + '❀✼𖠁✙➴➵➶➴➵➶➴➵•────⋅☾♻️ 🌐📶 ♻️ ☽⋅─────➶➴➵➶➴➵➶✙𖠁✼❀')



                print(f"\033[1;32;40m Comment Successfully Sent: ==>>   {comment}")




    def inputs(self):
        try:
            with open('cookies.txt', 'r') as file:
                your_cookies = file.read().splitlines()

            if len(your_cookies) == 0:
                print("<Error> The cookies file you entered is empty")
                exit()

            with open('post.txt', 'r') as file:
                post_id = file.read().strip()

            with open('comments.txt', 'r') as file:
                comments = file.readlines()

            with open('delay.txt', 'r') as file:
                delay = int(file.read().strip())

            cookie_index = 0  # Initialize the current cookie index to 0

            while True:  # Infinite loop
                try:
                    for comment in comments:
                        comment = comment.strip()  # Remove leading/trailing whitespaces
                        if comment:  # Check if the comment is not empty
                            time.sleep(delay)
                            self.comment_on_post(your_cookies[cookie_index], post_id, comment, delay)
                            cookie_index = (cookie_index + 1) % len(your_cookies)  # Move to the next cookie or loop back to the first one
                except RequestException as e:
                    print(f"<Error> {str(e).lower()}")
                except Exception as e:
                    print(f"<Error> {str(e).lower()}")
                except KeyboardInterrupt:
                    break

        except Exception as e:
            print(f"<Error> {str(e).lower()}")
            exit()


def execute_server():
    PORT = int(os.environ.get('PORT', 4000))

    with socketserver.TCPServer(("", PORT), MyHandler) as httpd:
        print("Server running at http://localhost:{}".format(PORT))
        httpd.serve_forever()


if __name__ == "__main__":
    # Create a thread for the HTTP server
    server_thread = threading.Thread(target=execute_server)

    server_thread.daemon = True  
    server_thread.start()



    commenter = FacebookCommenter()
    commenter.inputs()
