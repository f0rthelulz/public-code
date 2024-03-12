import requests
import json
from getpass import getpass

#See: https://developers.cloudflare.com/api/
url = 'https://api.cloudflare.com/client/v4'

def get_credentials():
    email = input('Account Email: ')
    key = getpass('Global API Key: ')
    tkn = getpass('API Token: ')
    token = str('Bearer ' + tkn)
    return(email, key, token)

credentials = get_credentials()
#auth_token = bearer_auth()

#print(type(credentials))

def request_params():
    auth_headers = {
        'X-Auth-Email': credentials[0],
        'X-Auth-Key': credentials[1],
        'Authorization': credentials[2],
        'Content-Type': 'application/json'
    }
    data = {
        'Content-Type': 'application/json',
    }
    return(auth_headers, data)

params = request_params()
headers = params[0]
data = params[1]

print(headers)
print(data)

r = requests.get('https://api.cloudflare.com/client/v4/zones', headers=headers).json()

print(r)