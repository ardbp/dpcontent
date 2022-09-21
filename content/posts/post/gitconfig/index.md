---
title: "Git Config"
date: 2022-09-19T19:01:48+08:00  
lastmod: 2022-09-19T19:01:48+08:00
draft: true
tags: ["git", "config", "github"]
categories: ["git"]
author: "Daxesh Panchal"

autoCollapseToc: true
# contentCopyright: '<a href="https://github.com/gohugoio/hugoBasicExample" rel="noopener" target="_blank">See origin</a>'

---

# **Git configuration command**
Git config allows you to set various configuration options for git.

## **Setting global username and email.**
      git config --global user.name ardbp
      git config --global user.email panchaldaxesh@gmailc.om

## **Add new remote url to git**
      git remote add origin https://github.com/ardbp/grpctemplate.git

## **Updating remote url to git**
      git remote set-url origin https://github.com/ardbp/grpctemplate.git 

      or

      git remote set-url origin git@github.com:ardbp/grpctemplate.git 

## **Display remote url**
      git remote -v

## **Using ssh over https using git config**
      git config --global url."git@github.com:".insteadOf "https://github.com/"
