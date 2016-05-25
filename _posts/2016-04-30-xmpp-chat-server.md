---
id: 120
title: Creating XMPP chat server
date: 2016-04-30T10:02:06+00:00
author: Ashutosh
layout: post
guid: http://aboutashu.com/blog/?p=120
permalink: /xmpp-chat-server/
views:
  - 33
dsq_thread_id:
  - 4788956514
categories:
  - Uncategorized
---
## Creating XMPP chat server

In this part of series, I am going to show you how to create an XMPP chat server.
  
To build your own XMPP based chat solution in android, you need to have two important things:

  1. XMPP based chat server
  2. XMPP integration in your android code

XMPP chat server or XMPP server is the one that handles chat database, configurations, user, options etc. Basically, it is a chat server that handles your chat backend. What remains is to implement the chat in your own language and have fun.

There are many variations of XMPP chat servers available today. You can use any of them depending on your requirements and available configurations. What I would recommend are [openfire](http://www.igniterealtime.org/projects/openfire/) and [ejabberd](https://www.ejabberd.im/).

Personally, I am going to use ejabberd to setup XMPP chat server. Let&#8217;s get into action. For now, I am using [DigitalOcean VPS](https://www.digitalocean.com) with Ubuntu Server 14.04 LTS installed, to host the server. You can test it with your own localhost machine.

  1. Connect to your ubuntu box using SSH. This will bring you to console screen. If you are using ubuntu in your local machine, you can safely skip this step.
  2. Run the command <span class="lang:default decode:true crayon-inline">sudo apt-get update</span> to update any packages.
  3. Enter following command to install ejabberd to your ubuntu powered machine:
  
    [code]sudo apt-get -y install ejabberd[/code]</p> 
    This will install ejabberd and will also create a configuration file which you can edit to have your own jabberd configuration. We will be editing the file in below step.</li> 
    
      * Time to edit the configuration file now. Enter the following command to bring up nano editor <span class="lang:default decode:true crayon-inline ">sudo nano /etc/ejabberd/ejabberd.cfg</span> . You&#8217;ll see a lot of text there and lot of options. Don&#8217;t worry. You only have edit 3-4 options overall. I&#8217;ll be doing them below, so pay attention.
      * In the nano screen, you will see the configuration file as shown below <div id="attachment_130" style="width: 1336px" class="wp-caption aligncenter">
          <a href="http://aboutashu.com/blog/wp-content/uploads/2016/04/nano.png"><img class="wp-image-130 size-full" title="ejabberd.cfg" src="http://aboutashu.com/blog/wp-content/uploads/2016/04/nano.png" alt="ejabberd.cfg" width="1326" height="619" srcset="http://107.170.45.123/blog/wp-content/uploads/2016/04/nano.png 1326w, http://107.170.45.123/blog/wp-content/uploads/2016/04/nano-300x140.png 300w, http://107.170.45.123/blog/wp-content/uploads/2016/04/nano-768x359.png 768w, http://107.170.45.123/blog/wp-content/uploads/2016/04/nano-1024x478.png 1024w" sizes="(max-width: 1326px) 100vw, 1326px" /></a>
          
          <p class="wp-caption-text">
            ejabberd configuration file
          </p>
        </div>
        
        Use your cursor to move below and find the line which has this text:
        
        <pre class=""><code>%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%
%% Options which are set by Debconf and managed by ucf

%% Admin user
{acl, admin, {user, "admin", "localhost"}}.

%% Hostname
{hosts, ["localhost"]}.

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%</code>
</pre>
        
        `{acl, admin, {user, "admin", "localhost"}}.` This line tells that user with username &#8220;admin@localhost&#8221; is admin of ejabberd configuration. We can add multiple users here to make them admin. Let&#8217;s add one. So, press enter key and add similar line with different username.
        
        <span class="lang:default decode:true crayon-inline ">{acl, admin, {user, &#8220;myusername&#8221;, &#8220;localhost&#8221;}}.</span>
        
        %% Hostname defines the server at which ejabberd should listen and connect. If you are not using a local machine (i.e you are using VPS such as DigitalOcean) to host the XMPP chat server, you can mention the domain name or IP address here. I am assuming example.com as my VPS domain name, hence I will modify this line as:
        
        <pre class=""><code>%% Hostname
{hosts, ["localhost","example.com"]}.</code></pre>
        
        Thats all the changes you have to make for now. Once you have made the above changes, the cfg file will now look like:
        
        <pre class=""><code>%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%
%% Options which are set by Debconf and managed by ucf

%% Admin user
{acl, admin, {user, "admin", "localhost"}}.
</code><code>{acl, admin, {user, "myusername", "localhost"}}

</code><code> %% Hostname 
{hosts, ["localhost","example.com"]}. 

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%</code></pre>
    
      * Now exit the nano editor by pressing CTRL + X. This will prompt you if you want to overwrite the previous cfg file, press &#8220;Y&#8221;, then press Enter again to confirm. This will bring you back to terminal screen.
      * We have mentioned that &#8220;myusername&#8221; is another user holding administrative rights to ejabberd, but we still didn&#8217;t mentioned his password. User &#8220;myusername&#8221; has to be registered to XMPP server, right? Lets register him as well. Enter following command to register: <pre class="">ejabberdctl register myusername localhost password
</pre>
        
        Replace &#8220;password&#8221; with whatever password you want to use to login. Once you are done with this, You have to re-start your ejabberd server by issuing this command:
        
        <pre class="">service ejabberd restart
</pre>
    
      * Fire up your public domain name in your favorite browser on port 5280 to login to ejabberd server: http://example.com:5280 .This will bring up webpage asking you for username and password. Login with username: myusername@localhost, password: whatever you mentioned in step 7.**NOTE**: If you are using local machine, your domain would be your local ip address. In windows, you can find this by executing _ipconfig _command in command prompt (It will be in the form 192.168.0.xxx), while for ubuntu, you can use _ifconfig. _
  
        [<img class="aligncenter size-full wp-image-131" src="http://aboutashu.com/blog/wp-content/uploads/2016/04/ejabberd-amin.png" alt="ejabberd amin" width="1366" height="561" srcset="http://107.170.45.123/blog/wp-content/uploads/2016/04/ejabberd-amin.png 1366w, http://107.170.45.123/blog/wp-content/uploads/2016/04/ejabberd-amin-300x123.png 300w, http://107.170.45.123/blog/wp-content/uploads/2016/04/ejabberd-amin-768x315.png 768w, http://107.170.45.123/blog/wp-content/uploads/2016/04/ejabberd-amin-1024x421.png 1024w" sizes="(max-width: 1366px) 100vw, 1366px" />](http://aboutashu.com/blog/wp-content/uploads/2016/04/ejabberd-amin.png)
      * Once logged in, you&#8217;ll see admin panel like this[<img class="aligncenter size-full wp-image-132" src="http://aboutashu.com/blog/wp-content/uploads/2016/04/ejabberd-admin-2.png" alt="ejabberd admin portal" width="1366" height="364" srcset="http://107.170.45.123/blog/wp-content/uploads/2016/04/ejabberd-admin-2.png 1366w, http://107.170.45.123/blog/wp-content/uploads/2016/04/ejabberd-admin-2-300x80.png 300w, http://107.170.45.123/blog/wp-content/uploads/2016/04/ejabberd-admin-2-768x205.png 768w, http://107.170.45.123/blog/wp-content/uploads/2016/04/ejabberd-admin-2-1024x273.png 1024w" sizes="(max-width: 1366px) 100vw, 1366px" />](http://aboutashu.com/blog/wp-content/uploads/2016/04/ejabberd-admin-2.png)</ol> 
    
    &nbsp;
    
    You have now successfully installed ejabberd on your machine and can be used to connect to any XMPP based chat client. In the next post, you&#8217;ll learn how to implement XMPP chat client in android code. Have fun!!
    
    Thanks for going threw entire long post. Please like and share if you like the article. Ask anything in comments below.
    
    ### Troubleshooting:
    
    Sometimes, ejabberd might not open up in your browser and will give &#8220;Connection timed out&#8221; error. It usually happens if firewall is blocking ports on which ejabberd relies for communication stuffs. Usually those ports are 5222, 5269, 5280. You can try opening those ports like I did below:
    
      1. Enter following commands in terminal: 
            iptables -A INPUT -p tcp --dport 5222 -j ACCEPT
            iptables -A INPUT -p tcp --dport 5269 -j ACCEPT
            iptables -A INPUT -p tcp --dport 5280 -j ACCEPT
    
      2. Open up **/etc/hosts** file by <span class="lang:default decode:true crayon-inline ">sudo nano /etc/hosts</span>  and add your domain name into it (assuming your public ip address is 123.123.10.210) : 
            123.123.10.210 example.com
    
      3. Allow ubuntu firewall 
            sudo ufw allow 5222
            sudo ufw allow 5269
            sudo ufw allow 5280
        
        &nbsp;</li> </ol> 
        
        This will open the ports and allow public connections, which should fix the issue.