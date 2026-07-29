# [Drupal - Discovery & Enumeration](Drupal%20-%20Discovery%20&%20Enumeration.md)
### Identify the Drupal version number in use on http://drupal-qa.inlanefreight.local

```shell
curl -s http://drupal-qa.inlanefreight.local/CHANGELOG.txt | grep -m2 ""

Drupal 7.30, 2014-07-24
```
