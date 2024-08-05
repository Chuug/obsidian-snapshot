
https://tryhackme.com/r/room/snappedphishingline

---

I mainly use [Phishtool](https://app.phishtool.com/) to get information about the emails

**Q1. Who is the individual who received an email attachment containing a PDF?**
`Willian McClean` is the one to get the `Quote.pdf` file with the `367a1c742a3af30f1c9c7de6ef80200c` sum => [VirusTotal](https://www.virustotal.com/gui/file/04ae3286641e71356ab3fb8e05cee0da58d94a7f6afe49620d24831db33d3441)

**Q2. What email address was used by the adversary to send the phishing emails?**
`Accounts.Payable@groupmarketingonline.icu` is the one sending the mail

The domain is on [VirusTotal](https://www.virustotal.com/gui/domain/groupmarketingonline.icu)

**Q3. What is the redirection URL to the phishing page for the individual Zoe Duncan? (defanged format)**

I used VsCode to get the base64 encoded `Direct Credit Advice.html` which gave me the url

```text
http://kennaroads.buzz/data/Update365/office365/40e7baa2f826a57fcf04e5202526f8bd/?email=zoe.duncan@swiftspend.finance&error
```

**Q4. What is the URL to the .zip archive of the phishing kit? (defanged format)**

For this one, I used my imagination like Spongebob and then found

**Q5. What is the SHA256 hash of the phishing kit archive?**

The kit were on the VM, fuck me. Downloaded it and `sha256sum` on the zip archive

The address on the VM is `http://kennaroads.buzz/data/Update365.zip`

**Q6. When was the phishing kit archive first submitted? (format: YYYY-MM-DD HH:MM:SS UTC)**

With the hash, I am able to find it on [VirusTotal](https://www.virustotal.com/gui/file/ba3c15267393419eb08c7b2652b8b6b39b406ef300ae8a18fee4d16b19ac9686/details) and find the `First Submission` date

**Q7. When was the SSL certificate the phishing domain used to host the phishing kit archive first logged? (format: YYYY-MM-DD)**

I put `kennaroads.buzz` on [crt.sh](https://crt.sh/?q=kennaroads.buzz)

**Q8. What was the email address of the user who submitted their password twice?**

Still on the VM on `http://kennaroads.buzz/data/Update365/log.txt`, we can see the one

**Q9. What was the email address used by the adversary to collect compromised credentials?**

Found it in `resubmit.php` file, line 47

**Q10. The adversary used other email addresses in the obtained phishing kit. What is the email address that ends in "@gmail.com"?**

Easy one, everywhere in the files

**Q11. What is the hidden flag?**

After like an eternity, I finally went on `http://kennaroads.buzz/data/Update365/office365/delete.php` which gave me the simple link to the `flag.txt`

It gave me a base64 encoded string I had to reverse
```shell
echo '}LRU_3Ht_hT1w_y4Lp{MHT' | rev
```

gg