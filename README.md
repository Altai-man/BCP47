# Example

A simple Perl 6 module for processing BCP-74 codes.

    use Intl::BCP47;
    my $tag = LanguageTag.new("oc-Latn-ES-aranes-t-en-UK");

    say $tag.language.code;   #  ↪︎ "oc"
    say $tag.variant[0].code; #  ↪︎ "aranese"

    my $not-pretty-tag = LanguageTag.new("eN-lATn-Us-u-ca-gregory-t-es-MX");
    say $not-pretty-tag.canonical; #  ↪︎ "en-Latn-US-t-es-MX-u-ca-gregory"

You can also filter a set of codes by passing in a tag-like filter:

    my @tags = (
      LanguageTag.new('es-ES'),
      LanguageTag.new('es-MX'),
      LanguageTag.new('es-GQ'),
      LanguageTag.new('pt-GQ'),
    );

    filter-language-tags: @tags, 'es';   #  ↪︎ [es-ES], [es-MX], [es-GQ]
    filter-language-tags: @tags, '*-GQ'; #  ↪︎ [es-GQ], [pt-GQ]

Or to check a single value, you can also smart match on a filter object:

    my $filter = LanguageTagFilter('*-Latf'); # 'in Fraktur script'

    LanguageTag.new('de-Latf-DE') ~~ $filter # ↪︎ True
    LanguageTag.new('de-Latn-AU') ~~ $filter # ↪︎ False

The filtering is based on RFC4647 and is what you might expect in a HTTP request
header.  However, for the ambitious, the special LanguageTagFilter object
provides for a good more flexibility and power than what you get from the basic
new(Str).

# Supported Standards

Intl::BCP47 implements BCP47, which defines the structure of language tags.

# To do

Right now the module processes tags and provides handy .gist methods and basic
support for canonicalization, but that can be improved.

Future versions will provide more robust error handling (with warnings for
deprecated or undefined tags), support for irregular grandfathered tags, support
for preferred forms of both standard and grandfathered tags, and better
documentation.

Additional support will later be given for handling the two defined extensions
because at the moment, their canonical forms merely parrot back the source
form (but placing -t before -u) without adjusting internal order or
capitalization.

Once additional elements of the CLDR are integrated into other Perl 6 modules,
then support may be added for more descriptive readouts of the tags (e.g.,
saying

    say LanguageTag.new("en-Shaw-US-t-es-Hebr").description

would say "English from the United States in the Shaw script which was transformed
(translated) from Spanish written in the Hebrew script" (or similar verbiage).
If we get more ambitious (and I plan on it!), given a $LANG environment variable
 set to 'ast', the result would be "inglés de los Estaos Xuníos con
calteres latinos que se tornó de castellanu escritu con calteres hebreos".

# License

All files (unless noted otherwise) can be used, modified and redistributed
under the terms of the Artistic License Version 2. Examples (in the
documentation, in tests or distributed as separate files) can be considered
public domain.
