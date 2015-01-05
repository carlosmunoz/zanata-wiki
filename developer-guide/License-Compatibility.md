A quick-reference for developers checking whether a library can be linked/copied in Zanata under the current license.

### Disclaimer

This page is intended only as a quick-reference to speed up investigating licenses for libraries used in Zanata. It does not constitute legal advice, and license decisions should only be made with appropriate investigation and legal advice.


## Zanata's License

Zanata uses LGPLv2.1.

See [LGPLv2.1 at gnu.org](http://www.gnu.org/licenses/lgpl-2.1.html)


## Library License Compatibility

In the diagrams below, licenses with arrows pointing to LGPLv2.1 can probably be used in Zanata.


### Compatible

 - Public Domain
 - [MIT / X11 License](http://opensource.org/licenses/MIT)
 - [ISC License](http://www.isc.org/downloads/software-support-policy/isc-license/)
 - Modified BSD License
 - zlib License
 - Expat License
 - LGPLv2.1+

 - LGPLv3/3+ (for linking only)

### Incompatible

I haven't checked these in detail. It is possible that linking libraries may be ok under some of these licenses.

 - XFree86 1.1 License
 - [Apache License 2.0](http://www.apache.org/licenses/LICENSE-2.0.html)
 - [Mozilla Public License](http://www.mozilla.org/MPL/)
 - GPLv2/2+/3/3+
 - Affero GPLv3




### Diagrams

[Compatibility matrix for GPL/LGPL code](https://fedoraproject.org/wiki/Licensing:Main?rd=Licensing#GPL_Compatibility_Matrix)

-----

[![License compatibility according to gnu.org](http://www.gnu.org/licenses/quick-guide-gplv3-compatibility.png)](http://www.gnu.org/licenses/quick-guide-gplv3.html#new-compatible-licenses)

source: http://www.gnu.org/licenses/quick-guide-gplv3.html#new-compatible-licenses

-----

[![License compatibility according to David Wheeler, 2007](http://www.dwheeler.com/essays/floss-license-slide-image.png)](http://www.dwheeler.com/essays/floss-license-slide.html)

source: http://www.dwheeler.com/essays/floss-license-slide.html

