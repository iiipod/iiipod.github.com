---
title: Emerge Error 0x01
layout: post
author:
  name: macint0sh
---
Can't locate Locale/Messages.pm in @INC (@INC contains: /usr/share/texinfo /etc/perl /usr/local/lib/perl5/5.16.3/i686-linux /usr/local/lib/perl5/5.16.3 /usr/lib/perl5/vendor_perl/5.16.3/i686-linux /usr/lib/perl5/vendor_perl/5.16.3 /usr/local/lib/perl5 /usr/lib/perl5/vendor_perl /usr/lib/perl5/5.16.3/i686-linux /usr/lib/perl5/5.16.3 .) at /usr/share/texinfo/Texinfo/Report.pm line 49.<br>
BEGIN failed--compilation aborted at /usr/share/texinfo/Texinfo/Report.pm line 49.<br>
Compilation failed in require at /usr/share/texinfo/Texinfo/Parser.pm line 50.<br>
BEGIN failed--compilation aborted at /usr/share/texinfo/Texinfo/Parser.pm line 50.<br>
Compilation failed in require at /usr/bin/makeinfo line 101.<br>
BEGIN failed--compilation aborted at /usr/bin/makeinfo line 101.<br>
make[2]: *** [dc.info] Error 2<br>
make[2]: Leaving directory \`/var/tmp/portage/sys-devel/bc-1.06.95-r1/work/bc-1.06.95/doc\'<br>
make[1]: *** [all-recursive] Error 1<br>
make[1]: Leaving directory \`/var/tmp/portage/sys-devel/bc-1.06.95-r1/work/bc-1.06.95\'<br>
make: *** [all] Error 2<br>
emake failed<br>
 * ERROR: sys-devel/bc-1.06.95-r1::gentoo failed (compile phase):<br>
 *   emake failed<br>
 *<br>
 * Call stack:<br>
 * ebuild.sh, line   93:  Called src_compile<br>
 * environment, line 2420:  Called __eapi2_src_compile<br>
 * phase-helpers.sh, line  688:  Called die<br>
 * The specific snippet of code:<br>
 * emake || die "emake failed"<br>
++++++++++++++++++++++++++++++++++++++++++++++++++++
## \#perl-cleaner --all
It is kind of like revdep-rebuild, but then for Perl packages; once you upgrade Perl, you need to run \`perl-cleaner --all\` in order for the modules to get rebuild for the new Perl (and the old ones to get removed).[http://www.gentoo.org/proj/en/perl/perl-cleaner.xml](http://www.gentoo.org/proj/en/perl/perl-cleaner.xml)

