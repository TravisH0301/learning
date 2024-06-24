# Authenticating On-Premise SharePoint API
1. [Assumption](#Assumption)
2. [High-level Overview on Mechanism](#High-level-Overview-on-Mechanism)
3. [Example Code](#Example-Code)
SharePoint provides two versions; SharePoint Online & SharePoint On-Premise.
In authenticating connection to the SharePoint API, it requires a different approach for the SharePoint hosted on-prem environment.
This note shows steps to authenticate using Python when the on-prem environment uses NTLM & AD FS for authentication.

- NTLM: NT (New Technology) LAN Manager (NTLM) is a suite of Microsoft security protocols intended to provide authentication to users in a Windows network.
- AD FS: Active Directory Federation Service (AD FS) enables Federated Identity and Access Management by securely sharing digital identity and entitlements rights in a single security or enterprise boundary - Single sign-on.

## Assumption
This assumes the SharePoint is managed on-prem and the network is Windows using AD FS. For SharePoint Online, please refer to [Office365-REST-Python-Client](https://pypi.org/project/Office365-REST-Python-Client/).

## High-level Overview on Mechanism
1. Client (Python) Sends GET Request to on-prem SharePoint to Initiate NTLM Authentication:
- The client sends an initial GET request to the SharePoint URL.
- This request initiates the NTLM authentication process.

2. requests_ntlm Module Takes Care of NTLM Authentication with the Given Credentials:
- The requests_ntlm library handles the NTLM handshake automatically.
- It responds to the server's NTLM challenge with the appropriate credentials.

3. Upon Successful NTLM Authentication, Server Redirects Client to the IdP (AD FS) with the Token:
- After successful NTLM authentication, the server redirects the client to the Identity Provider (IdP), which in this case is AD FS.
- This redirection typically includes a token or other authentication information in the response.

4. Client Sends POST Request with the Given Redirection and Token Information to Finally Authenticate with the AD FS:
- The client extracts the necessary tokens (like wctx and wresult) from the redirection response.
- The client then sends a POST request to the IdP with these tokens to complete the authentication process.

5. The Session Contains Federated Authentication Cookie and Python Can Use This Cookie to Authenticate Its Connection to SharePoint Service:
- Upon successful authentication, the server sets a federated authentication cookie in the session.
- The client (Python script) can now use this session, which includes the authentication cookie, to make authenticated requests to the SharePoint service.

## Example Code
    import requests
    from requests_ntlm import HttpNtlmAuth
    from bs4 import BeautifulSoup
    from urllib.parse import urlencode
    
    # Define NTLM authentication credentials
    username = "<LAN ID>"
    password = "<password>"
    domain = "<domain>"
    
    # Create a session
    session = requests.Session()
    
    # Set up NTLM authentication
    session.auth = HttpNtlmAuth(f"{domain}\\{username}", password, session)
    
    # Define headers
    headers = {
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.103 Safari/537.36",
    }
    
    # Initial GET request to On-prem SharePoint
    url = "https://<On-Prem SharePoint URL>/<Project or Team URL>/_layouts/authenticate.aspx"
    response = session.get(url, headers=headers, verify=False)    

    # Parse the response to find authentication form
    soup = BeautifulSoup(response.text, "html.parser")
    form = soup.find('form', {'name': 'hiddenform'})
    if form:
        adfs_url = form.get('action')
        wctx = form.find("input", {"name": "wctx"})["value"]
        wresult = form.find("input", {"name": "wresult"})["value"].replace("&quot;", '"').replace("&lt;", "<")
        payload = {
            "wctx": wctx,
            "wresult": wresult,
            "wa": "wsignin1.0"
        }
    
        # Submit the login form to the IdP
        response = session.post(adfs_url, headers=headers, data=payload, verify=False)
        cookie = "FedAuth=" + response.cookies.get_dict()["FedAuth"]

    # Add cookie to headers to authenticate future request to SharePoint
    headers["Cookie"] = self.__cookie
    headers["Content-Type"] = "application/json; odata=verbose"
    headers["Accept"] = "application/json; odata=verbose"
