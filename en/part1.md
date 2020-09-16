# 1. Background

This document describes http2 from a technical and protocol level. It started
out as a presentation Daniel did in Stockholm in April 2014 that was
subsequently converted and extended into a full-blown document with all
details and proper explanations.

RFC 7540 is the official name of the final http2 specification and it was published on May 15th 2015: https://www.rfc-editor.org/rfc/rfc7540.txt

All and any errors in this document are my own and the results of my
shortcomings. Please point them out and they will be fixed in updated
versions.

In this document I've tried to consistently use the word "http2" to describe
the new protocol while in pure technical terms, the proper name is HTTP/2. I
made this choice for the sake of readability and to get a better flow in the
language.

## 1.1 Author

My name is Daniel Stenberg. I've been working with open source and networking
for over twenty years in numerous projects. Possibly I'm best known for being
the lead developer of curl and libcurl. I've been involved in the IETF HTTPbis
working group for several years and there I've kept up-to-date with the
refreshed HTTP 1.1 work as well as being involved in the http2 standardization
work.

  Email: daniel@haxx.se

  Twitter: [@bagder](https://twitter.com/bagder)

  Web: [daniel.haxx.se](https://daniel.haxx.se/)

  Blog: [daniel.haxx.se/blog](https://daniel.haxx.se/blog/)

## 1.2 Help!

If you find mistakes, omissions, errors or blatant lies in this document, please send me a refreshed version of the affected paragraph and I'll make amended versions. I will give proper credits to everyone who helps out! I hope to make this document better over time.

This document is available at [https://daniel.haxx.se/http2](https://daniel.haxx.se/http2)

## 1.3 License

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/creative-commons.png" />

This document is licensed under the Creative Commons Attribution 4.0 license: https://creativecommons.org/licenses/by/4.0/

## 1.4 Document history

The first version of this document was published on April 25th 2014. Here follows the largest changes in the most recent document versions.

### Version 1.13

- Converted the master version of this document to Markdown syntax
- 13: Mention more resources, updated links and descriptions 
- 12: Updated the QUIC description with reference to draft 
- 8.5: Refreshed with current numbers 
- 3.4: The average is now 40 TCP connections 
- 6.4: Updated to reflect what the spec says 

### Version 1.12

- 1.1: HTTP/2 is now in an official RFC 
- 6.5.1: Link to the HPACK RFC 
- 9.1: Mention the Firefox 36+ config switch for http2 
- 12.1: Added section about QUIC 

### Version 1.11

- Lots of language improvements mostly pointed out by friendly contributors 
- 8.3.2: Mention nginx and Apache httpd specific acitivities 

### Version 1.10

- 1: The protocol has been “okayed” 
- 4.1: Refreshed the wording since 2014 is last year 
- Front: Added image and call it “http2 explained” there, fixed link 
- 1.4: Added document history section 
- Many spelling and grammar mistakes corrected 
- 14: Added thanks to bug reporters 
- 2.4: Better labels for the HTTP growth graph 
- 6.3: Corrected the wagon order in the multiplexed train 
- 6.5.1: HPACK draft-12 

### Version 1.9

- Updated to HTTP/2 draft-17 and HPACK draft-11  
- Added section "10. http2 in Chromium" (== one page longer now)  
- Lots of spell fixes  
- At 30 implementations now  
- 8.5: Added some current usage numbers  
- 8.3.1: Mention internet explorer too  
- 8.3.2: Added "missing implementations"  
- 8.4.4: Mention that TLS also increases success rate
