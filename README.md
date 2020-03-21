# dojo-cloud
A streamlined deployment of [Samourai Dojo](https://samouraiwallet.com/dojo) on AWS EC2

This is *not* officially endorsed or supported by Samourai Wallet.

## Summary
Dojo is a software stack providing a full Samourai backend, enabling users to run Samourai apps without trusting Samourai's infrastructure.

This guide aims to make it easy to run Dojo on public cloud platforms—just AWS for now, but maybe others someday if it seems useful. While Dojo is typically deployed on hardware you physically control, AWS may be attractive if...
* You just want to test it out
* You travel often
* You don't have or or want physical stuff
* You're on the run, fleeing from violent nocoiners

When it comes to security, each individual's circumstance is so unique and hard to predict that having a variety of options with explicit tradeoffs is generally nice.

If you'd rather run Dojo locally instead (great!), check out [Ronin Dojo](https://code.samourai.io/ronindojo).

## How it Works
An Amazon Machine Image (AMI) has been built that includes automation to fully automate the Dojo deployment. Simply launch a new virtual machine, select that image and some default values, and the Dojo setup happens automatically. Passwords for the Dojo services, which should be secure and unique, are randomly generated at deployment time. The onion addresses you'll need in order to pair your mobile wallet and access the services remotely are dropped in a text file once setup is complete.

Note that the Bitcoin blockchain is downloaded during installation, which can take a while—up to a couple days, depending on the instance specs.

## Deployment Instructions
1. Login to [AWS](https://portal.aws.amazon.com/billing/signup?nc2=h_ct&src=default&redirect_url=https%3A%2F%2Faws.amazon.com%2Fregistration-confirmation#/start)
2. Navigate to EC2
3. Select the Oregon (us-west-2) region
4. Select launch a new instance
5. Select **AMI-0aa8514b316a6e37a**
6. Customize whatever you like, but the defaults should be fine as long as the specs meet the Dojo minimum requirements (detailed below).

The default security group, which only allows SSH, should be fine since all the other services communicate through Tor.

## Privacy and Security Considerations
Running Dojo on Amazon entails some security tradeoffs.

Most importantly, your Bitcoin private keys live only on the Samourai mobile app and are _never_ shared with Dojo, so neither Samourai (the company) nor Amazon ever see those.

Dojo does, however, require access to your XPUB. If someone else were to gain access to your XPUB, they couldn't steal funds but could view all of your wallet addresses and transaction history. When using Samourai Wallet with no Dojo in the picture at all, your XPUB is stored on Samourai's servers. When running Dojo locally, everything is on hardware you physically control. AWS offers an in-between, with your XPUB being on Amazon's hardware that you virtually control.

Concerns about surveillance at the network and ISP level are minimal since Dojo routes all traffic through [Tor](https://www.torproject.org/).

## Cost
Cost depends on a few factors, primarily the instance specs such as CPU, RAM, and disk. 

**Minimum (default, ~$75/month)**
* t3a.medium
* 2 vCPUs
* 4GB RAM
* 500GB SSD EBS volume

EC2 instances are "on-demand" by default, meaning you pay a fixed hourly rate while the instance runs and can terminate it any time. If you plan to run Dojo on AWS for many months, considering using a [reserved instance](https://aws.amazon.com/ec2/pricing/reserved-instances/) to lock in a better rate (sometimes up to 75% off).

For a full and authoritative list of costs, check out Amazon's [EC2 pricing](https://aws.amazon.com/ec2/pricing/on-demand/) and [cost calculator](https://calculator.s3.amazonaws.com/index.html).

## Future
If this proves useful, it could be expanded to...
* include more cloud providers such as Google Cloud and Microsoft Azure
* have the initial block download included in the image, to make it faster to get going
