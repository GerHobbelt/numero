# Numero

> Numero is a library for converting between Arabic numbers and their English numeral.
 
`numero` is a library which parses/extracts a decimal number, either integer or floating-point, either in scientific notation or not, from the given input string. It uses the thousands and decimal separator symbols given in the conversion options. 

It also provides methods for the inverse operation, i.e. converting a value to a (English) human-readable text, e.g. `"one thousand and twenty four"` for `1024`.


## Example usage / code


```
    num::conversion_options_t english_options;
    english_options.thousands_separator_symbol = ',';
    english_options.decimal_separator_symbol = '.';
    num::converter_c english_converter(english_options);

    BOOST_CHECK(english_converter.is_number("1"));
    BOOST_CHECK(english_converter.is_number("1e3"));
    BOOST_CHECK(english_converter.is_number("8million") == false);
    BOOST_CHECK(english_converter.is_number("1,000,000"));

    // no support for Indian "crore", alas ;'-)
    BOOST_CHECK(english_converter.is_number("1,00,000") == false);

    BOOST_CHECK(english_converter.is_number("0.333333"));
    BOOST_CHECK(english_converter.is_number("-6.25e-2"));

    num::conversion_options_t german_options;
    german_options.thousands_separator_symbol = '.';
    german_options.decimal_separator_symbol = ',';
    num::converter_c german_converter(german_options);

    BOOST_CHECK(german_converter.is_number("1.000.000"));
    BOOST_CHECK(german_converter.is_number("0,333333"));
    BOOST_CHECK(german_converter.is_number("-6,25e-2"));

    num::converter_c converter;

    BOOST_CHECK_THROW(converter.to_number("gazillion"), std::invalid_argument);

    BOOST_CHECK(converter.to_number("zero") == "0");
    BOOST_CHECK(converter.to_numeral("0") == "zero");

    BOOST_CHECK(converter.to_number("thirteen") == "13");
    BOOST_CHECK(converter.to_numeral("13") == "thirteen");

    BOOST_CHECK(converter.to_number("minus fifty-six") == "-56");
    BOOST_CHECK(converter.to_numeral("-56") == "negative fifty-six");

    BOOST_CHECK(converter.to_number("negative sixty-six") == "-66");
    BOOST_CHECK(converter.to_numeral("-66") == "negative sixty-six");

    BOOST_CHECK(converter.to_number("a hundred") == "100");
    BOOST_CHECK(converter.to_number("one hundred") == "100");
    BOOST_CHECK(converter.to_numeral("100") == "one hundred");

    BOOST_CHECK(converter.to_number("nineteen hundred") == "1,900");
    BOOST_CHECK(converter.to_numeral("1,900") == "one thousand nine hundred");

    BOOST_CHECK(converter.to_number("three trillion") == "3,000,000,000,000");
    BOOST_CHECK(converter.to_numeral("3,000,000,000,000") == "three trillion");

    BOOST_CHECK(converter.to_number("fifteen quindecillion") == "15,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000");
    BOOST_CHECK(converter.to_numeral("15,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000") == "fifteen quindecillion");
    
    BOOST_CHECK(converter.to_numeral("1e3") == "one thousand");
    BOOST_CHECK(converter.to_numeral("1e27") == "one octillion");
    BOOST_CHECK(converter.to_numeral("1.23e6") == "one million two hundred thirty thousand");

    BOOST_CHECK(converter.to_number("twelve million eighty-three thousand fifty-six") == "12,083,056");
    BOOST_CHECK(converter.to_numeral("12,083,056") == "twelve million eighty-three thousand fifty-six");

    BOOST_CHECK(converter.to_number("nine hundred ninety-nine thousand eleven") == "999,011");
    BOOST_CHECK(converter.to_numeral("999,011") == "nine hundred ninety-nine thousand eleven");
    
    converter.conversion_options().use_thousands_separators = false;

    BOOST_CHECK(converter.to_number("twelve million eighty-three thousand fifty-six") == "12083056");
    BOOST_CHECK(converter.to_number("nine hundred ninety-nine thousand eleven") == "999011");
    
    BOOST_CHECK_THROW(converter.to_number("six thousand fourty-four million"), std::logic_error);
    BOOST_CHECK_THROW(converter.to_number("six thousand twenty thousand ten"), std::logic_error);
    BOOST_CHECK_THROW(converter.to_number("six thousand seventeen hundred"), std::logic_error);
    BOOST_CHECK_THROW(converter.to_number("zero hundred"), std::logic_error);
    BOOST_CHECK_THROW(converter.to_number("four hundred three sixty"), std::logic_error);

    num::conversion_options_t options;
    options.naming_system = num::naming_system_t::long_scale;
    num::converter_c converter2(options);
    
    BOOST_CHECK(converter2.to_number("one milliard") == "1,000,000,000");
    BOOST_CHECK(converter2.to_numeral("1,000,000,000") == "one milliard");

    BOOST_CHECK(converter2.to_number("two billion") == "2,000,000,000,000");
    BOOST_CHECK(converter2.to_numeral("2,000,000,000,000") == "two billion");

    num::converter_c converter3;
    converter3.conversion_options().force_leading_zero = false;
    
    BOOST_CHECK(converter3.to_number("point zero six two five") == "0.0625");
    BOOST_CHECK(converter3.to_numeral("0.0625") == "point zero six two five");

    converter3.conversion_options().force_leading_zero = true;

    BOOST_CHECK(converter3.to_numeral("0.0625") == "zero point zero six two five");
```
