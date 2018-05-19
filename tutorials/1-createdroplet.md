# Create a Digital Ocean Droplet
These droplets are the Virtual Private Servers that are setup within minutes ready to go.

1. Use the referral code provided in the video and create your free Digital Ocean Account.
2. Click on the droplets menu and create a one click application for **LAMP 16.04 LTS**.
    - **LAMP** doesn't mean a physical lamp. **LAMP** stands for **Linux Apache MySQL and PHP**. The core ingredients to make a decent server stew.
    - For basic servers you can choose the standard minimum option for $5 a month. Compared to most hosting providers who can charge from a range of $45 (intro price) to $300 and above. Some of the hosting providers will give you an introductory rate and then raise the price almost 75% after the first year. It's mostly due to the fact that they maintain the server you are own and other services they provide in the background. If you aren't ready for big time hosting yet you don't have to spend an excess amount of dollars. With Digital Ocean they set standard prices and you get exactly what you need. If you choose to need more power, you can easily upgrade without the hassle of re-building your server.
    - Choose the 1GB Memory, 1 CPU, 25GB of Storage, and 1TB of transfer.
    - You can choose to have backups, which do cost per month because it uses storage space. If you are running a live wordpress site or something similar, it would be beneficial to enable this in case of corruption. However, if you only have a basic website you can easily rebuild then backups aren't necessary.
    - If you need more storage you can buy another volume if need be.
    - Choose your region, sometimes it's best to have it closer to where you live if you are going to make constant transfers. It's good for data speeds.
    - Enable Monitoring.
    - Name your Server. It's best to do something like **HBF-SERVER.example.com** if you have a domain name that you are going to attach to this. If not then just name it how you like but add **-example.com** to the end of your name. This is for FQDN purposes. Also remeber you cannot have any spaces so use hyphens and underscores as needed. You can add tags for faster searching if you have more than 5 or 10 servers.
    - Click create. It will take around 30 seconds or so. Sometimes longer depending on the network.

Go to install basic tools section next: ./2-basictools.md

Test link ../README.md
