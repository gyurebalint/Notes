1. The server software
    Okay, so first things first, you said ideally you want to be able to access documents and movies netflix style.
  
    For movies, of course there's Plex depending on the media format (.mkv, .mp4, .m4a etc) you have, depends on how well this will run on a raspberry pi. If plex has to transcode (translate the video format as you stream it), the CPU requirements shoot up, but it should work fairly well on a pi 4.
  
    For documents, I would recommend nextcloud. It's easy to setup and has instructions for a pi. It'll be fairly performant, and has a nice webUI, add ons and a frequent feature updates. I really rate it.
  
    Installing these two entails download software onto the pi (using sudo apt install etc), and there are loads of guides on the internet. Once that's done, you can think about how you'll expose your pi to the internet.
  2. Getting a dynamic IP address
    The first thing you need to consider when it comes to accessing your pi remotely is that you need a fixed point to connect to. The Global IP address your router is issued by your ISP will likely change. Not too frequently, but it's unpredictable and will be really annoying for you. You can purchase a static IP from ISPs, but often they are expensive and require a business connection too.
  
    The second option is a dynamic DNS (which I'll talk about in a minute), such as NoIP.com, and it's a great solution.
  
    With either option though, you will need to buy a domain name (a web address), this is done through a company called a registra, and there are many. Depending on which country you are in depends on which one you use, I personally rate namecheap, but you should do some research first.
  
    Once you've found a registra, buy a domain name you like, I'll use cfs3corsair.com as an example.
  
    Now you have a domain name, you will be able (through the registra's website) to setup DNS records, these are analogous to a telephone directory. DNS is the system which resolves website names to the underlying IP addresses.
  
    If you have a static IP address, then all you would have to do here is enter a DNS 'A' record (there are different types of records) to point to your static IP address. So if your static IP address was 9.105.83.83, then you would enter the DNS records:
  Record Type: A
Address: cfs3corsair.com 
Target: 9.105.83.83   
    If you are using a dynamic dns, then you would register with them, and install an agent on your server. This bit of software then tells the dynamic DNS company your IP address, and updates it every time it changes. The dynamic DNS company then give you an address, which you effectively forward your DNS requests to.
  
    It's like if you changed phone numbers once a week, so instead of updating everyone's contact address book every time it changes, you ask someone who's phone number doesn't change if  you can put their phone number in your friend's contact books, then when they phone him he gives them whatever number you are using that week.
  
    When using a dynamic DNS, you enter a 'cname' record into DNS. So say you use noip.com (there are other companies, you don't need to use them), they may give you the dynamic address R4e9F.noip.net, and the agent on your raspberry pi will ensure that noip know what IP address to use return for that address. But it can be updated much easier than normal DNS, which is why you use them. Then, in DNS for you put:
  Record Type: CNAME
Address: cfs3corsair.com 
Target: R4e9F.noip.net
    Et Voila! Now you have an internet address which always points to you router. Now, then fun part....
  3. Exposing yourself to the internet
    ( ͡° ͜ʖ ͡°) There are two main ways you can make pi accessible to you on the internet, and there are pros/cons to each method.
  VPN or SSH Tunneling
    Firstly you could use a VPN or SSH Tunnels, before I talk about the pros/cons, let's talk about what they do
  
    A VPN would allow you to connect from anywhere on the internet and act as though you are on your local network. I personally find that they're a bit tricky to setup properly, as they need to be secure. The other downside is that you need to have the VPN software and authentication keys on every device you connect from.
  
    SSH Tunnelling is similar-ish to a VPN, but instead of forwarding all your traffic to your pi, you can tunnel certain ports. You could even setup a proxy server like privoxy and SSH Tunnel the port, at which point you more or less have a VPN. For this method, you would want to ensure the port you use for SSH is not the default port (22). Don't get me wrong, security through obscurity is not really a valid method, so moving from the default port is less about being safe and more about cutting down the amount of brute force attacks you get.
  
    For SSH, you should disable root logins, and if you want set it to key-only authentication, so you need to have the keyfiles on the machine you are connecting from. The alternative to this is to use a two factor authentication PAM library on the raspberry pi, so you have to type in your username and then a one time password. This means you can now connect from any PC with SSH abilities as long as you have your phone with the second factor app on it (like Google Authenticator, or andOTP.
  Nginx and HTTPS
    The second option for exposing your services is to install a reverse proxy and setup SSL. This would let you access the login pages for your services just by visiting cfs3corsair.com in any browser. No software required.
  
    A reverse proxy (like NGINX) is a bit of software you can install on the pi which acts as a single point of entry to the pi, and then based on the hostname (cfs3corsair.com) or path (cfs3corsair.com/pathisthisbithere) it can route the incoming traffic to different ports (applications).
  
    HTTPS or SSL (actually TLS these days but don't worry about it), is how you can ensure the traffic (and therefore the usernames/passwords you submit) between you and your server are secure. Once you have your domain and Nginx setup you can easily get an SSL certificate using Lets encrypt
  
    So a VPN/SSH Tunnels are more secure, but require software and potentially keys on every machines you connect from, HTTPS using nginx means any web browser can connect, but you need to make sure the applications (like Nextcloud and Plex) have good security (those two do) as you are opening their login pages up to the world.
  
    That's quite a chunk of info, if you have any questions I'm happy to help, and if you let me know what option you want to go for I can give you more detail