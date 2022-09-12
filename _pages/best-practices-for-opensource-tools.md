---
layout: post
title: Best Practices for using OpenSource Tools in Enterprises
author: gini
categories: [ opensource ]
tags: [opensource, free software]
permalink: opensource-tools-best-practices
featured: false
hidden: false
image: "assets/images/tools-1426927308491-6380b6a9936f.jpeg"
titleshort: 
---

*These are purely personal notes - but publicly available - and do not blindly follow the items.*

- [Consider paid Opensource services as well](#consider-paid-opensource-services-as-well)
- [Be a contributor to the Community](#be-a-contributor-to-the-community)
- [Implement the best class Application monitoring System](#implement-the-best-class-application-monitoring-system)
- [Check in Vulnerability Database](#check-in-vulnerability-database)
- [Check the Long Term Support (LTS) before implementing](#check-the-long-term-support-lts-before-implementing)
- [Practice Software Inventory](#practice-software-inventory)
- [Implement Least Privilege Policies](#implement-least-privilege-policies)
- [Follow the Patching and Upgrade schedules](#follow-the-patching-and-upgrade-schedules)
- [Test, Stage and push to Production](#test-stage-and-push-to-production)
- [NO Default Passwords](#no-default-passwords)
- [Check the License of Opensource Tools](#check-the-license-of-opensource-tools)
- [Keep Source Codes in local repositories for scanning](#keep-source-codes-in-local-repositories-for-scanning)
- [Refereces](#refereces)

## Consider paid Opensource services as well
- Opensource doesn't mean everything free. Whatever community versions are free ($) but do consider paid optiions for better support.
- Paid services will be better in terms of QA, Stability, Release Cycle and Support.
- Organizations who are ready to take care of above items can think about community supported editions easily.

## Be a contributor to the Community
- Report bugs on time
- Raise feature requests and contribute back
- Be part of opensource development and testing team

## Implement the best class Application monitoring System
- make sure the applciation are under monitoring
- observability is the key 
- reporting mechanism and follow up tracking should be in place

## Check in Vulnerability Database
- Check in all Vulnerability databases available.
- Check in poduct Vulnerability page regularly.

## Check the Long Term Support (LTS) before implementing
- Make sure the tool is having long term support.
- Check the popularity and community strength.

## Practice Software Inventory
- Keep your software and tools database uptodate. 
- Keep the version information, Vulnerability tracker, upgrade options, End of Life (EOL)

## Implement Least Privilege Policies
- Allow access based on demand and request
- Block all access unless it is needed

## Follow the Patching and Upgrade schedules
- Do not wait for an issue 
- Upgrade and patch systems based on availability

## Test, Stage and push to Production
- Do not try anything in production as we do not know what all tests and QA has been completed from the opensource community level.
- Refer the release notes carfully before implementation/upgrade process.

## NO Default Passwords
- Change all default passwords to secure passwords (follow organization password policies)
- Keep passwords in vault or password manager.

## Check the License of Opensource Tools
- Make sure the license is acceptable as per organization policies
- Check what all data will be collected and send to external systems (if any) for improvement/analytics.

## Keep Source Codes in local repositories for scanning
- Scan the source code by yourself using tools
- Test fixes and bugs and propose to community

## Refereces
- [5 ways to keep open source-based apps secure](https://techbeacon.com/security/5-ways-keep-open-source-based-apps-secure)
- [Three Myths Debunked About Open Source Software Security](https://rubygarage.org/blog/open-source-software-security)
- [A Guide to Open-Source Software Security Risks & Best Practices](https://blog.ironbastion.com.au/open-source-software-security-risks-practices/)
