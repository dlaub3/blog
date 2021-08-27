---
quality: medium
author:
date: 2018-07-22
draft: false
title: "Practical Perl Primer"
description: "An intro to Perl"
tags: ["perl"]
categories: ["language basics"]
topic: "perl"
toc: true

---

## Documentation

```perl
# https://perldoc.perl.org/index-language.html
perldoc -f chomp # perldoc on chomp
perldoc perlfaq
```

## Misc

- The semicolon is technically a separator, but it is good practice to use it as a terminator at the end of each line of code. 

- Whitespace is mostly ignored.

## Variables and Values 

```perl
# represent a scalar value with $, Perl uses duck typing for scalar value
my $n = 42;

# represent arrays with @
my @array = qw(one two three four five);

# represent hashes with %
my %hash = (one => 1, two => 2, three => 3);

# a list is a fixed series of scalar values. 
# a list is not a data type, but it is used to create arrays and hashes in the examples above. 
# a list is created with "()". 
# a list is not mutable. 
# it's possible to create references to lists.
$list = ('one', 'one', 'three');


$var = 0 + $var; # force number 
$var = '' . $var; # force string
```

## Statements and Expressions 

```perl
# expressions represent values
my $var = 2;

# statements instruct computers on how to execute code
sub main {
  say "Hi";
}
```

## Assignment Examples 

```perl
my ($x, $y, $z) = (1, 2, 3); # assign multiple scalar variables from a list

my @array = (1, 2, 3); # create an array from a list

my $count = @array; # assign the length of the array
```

## Block and Scope

```perl
#!/usr/bin/perl
use 5.18.0;
use warnings;
use strict;

# if there is no block the scope is the whole file. Perl is block scoped.
my $var = 20; 

sub func {
  # $var is scoped locally to the code block
  my $var = 5;
}

my $num = 4;
if ($num > 3) {
  # $num2 is scoped to this code block 
  # since that is where it's defined
  my $num2 = 6;
  
  # $num can be used here 
  $num = $num + $num2;
}
# $num2 is out of scope
say $num2;
```

## Logical Values 

https://perlmaven.com/boolean-values-in-perl

```perl
# false values: '', 0, []  in general things that are empty or evaluate to 0
# everything else evaluates to true
# this is because of Perl's duck typing.
```

## Strings

```perl
my $s = "I am a string";
say "s is [$s]";

#string concatenation
$s .= " in a Perl expression";

#string interpolation
say "My name is String. $s .";
say "My name is String. \"$s\" .";
say "My name is String. ${s}.";
say qq(My name is String. $s .);
```

## Arrays

```perl
my @array = (one, two, three);
say foreach @array;
say $array[0]; # the $ is used to reference a scalar value in the array.

my $count = @array;
push @array, qw(one two three);
my $item = pop @array;

# slices represent part of the whole
say foreach @array[1...5];
my @arr2 = @array[1,6,3];

my $ref = [1,2,3]; # create a reference to an anonymous array.
my @array = qw( one, two, three, four );
my $ref = \@array; # create a reference to a named array.
say foreach @{$ref}; # dereference the reference into the array it references

```

## Hashes

```perl
# hash values are not stored in any predictable order

# create anonymous hash reference 
my $ref = {
    one => "one",
    two => "two"
};

# access element
${$ref}{one}
$ref->{one}

# create a hash
my %hash = (
	one => '1',
	two => 'two'
);

while ( my ($k, $v) = each %hash ) {
  say "$k -> $v";
}

foreach my $k (sort(keys %hash)) {
  my $v = $hash{$k};
  say "$k -> $v";
}

say foreach keys %hash;
say foreach values %hash;
```

## Constants  and Static Variables

```perl
# constants can be created in Perl as follows
use contant {
  PIE => 3.1415
  TRUE => 1, 
  FALSE => ''
};

# The use constant is the same as the sub below. 
sub PIE { 3.1415 };


# Perl 5.10 + support static variables 
use feature 'state';
state $n = 10; # Static variables will not be garbage collected for the entire runtime of the script. If you use it in a function, consecutive function calls will remember the value.
```

## Conditionals 

```perl
if ( $x == $y ) {
  $x = $y;
} elsif ( $x > $y ) {
  # note the spelling 'elsif'
 $x = 10;
} else {
  $x = 15;
}

# single line postifix if
# can't use else 
say 'true' if $x = 1;

# unless 
say 'true' unless $x = 1;

# Don't do this 'given' and switches are generally discouraged.
my $x;
my $y;
my $z;

given($v) {
  when ($x) { say 'x';}
  default { say 'default';}
}

# instead do this
my %hash = ( $x => 'x', $y => 'y', $z => 'z');
if ($hash{$x}) {
  say $hash{$v}
} else {
  say 'default';
}

# ternary if 
say $x > $y ? 'x' : 'y';
```

## Loops 

```perl
# iterate over a quote word list
say foreach  qw(one two three)

# while 
my @array = qw( one two three four five );
my $count = 0;
while(@array) {
  say shift @array;
  $y = shift @array;
  next if $y eq 20;
  last if $y eq  5;
} continue {
	$count++;
}

# for
for (my $i = 0; $arr[$i]; ++$i ) {
  say "$i: " . $arr[$i];
}

# foreach
foreach my $s (@arr) {
  say $s;
}
```

## Default Variables 

```perl
# https://perldoc.perl.org/perlvar.html
# http://www.perlmonks.org/?node_id=353259

foreach ( @arr ) {
  says $_;  # $_ optional value omitted will use the default variable $_
}

say foreach @arr;

func1('one', 'two');

sub func1 {
  say 'this is func1';
  say foreach @_;
  my ($a, $b) = @_;
  # push pop shift unshift all use the default array variable
}

# system error variable 
if ( -e $file ) {
  say "found";
} else {
  my $error = $!; # error variable 
  say $error;
}

# env variables 
foreach my $k (sort keys %ENV) {
	say "$k = $ENV{$K}"
}

say foreach @ARGV; # command-line arguments

my @numbers = qw( 3 2 6 5 8 7 9 2 3 7 );
my @numbers = sort {$a <=> $b} @numbers; # $a and $b are sort variables.

say $0   # path of script 
say $^o; # system name 
say $^V; # version of perl 

# autoflush variable, the system flushes the buffer at it's own discretion 
$| = 1; # this turns on autoflush 
```

## Operators 

```perl
# $x = 1 + 2;
# "" eq | ne ""
# 1 == 1
# % modulus returns the remainder of division

# compound assignment operator only evaluates once
my $x += 4;

## addition evaluates twice 
$x = $x + 5;

## relational Operators 
4 == 6
4 < 6
4 > 6
4 >= 6
(!4 || 57) {
    
}

# string operators
"str" eq "str"
"str" ne "str"
"str" lt "str"
"str" le "str" # less than or equal to
"str" gt "str"

## logical operators
use constant { TRUE => 1, FALSE => "" }
and ## ( true and true) == true
or  ## ( false and true) == true 
xor ## one true the other false  (false and true) == true
not ## ( not FALSE or not True) == true

## File test operators 
# see perldoc functions -X
# -s, non zero length
# -z, zero length
# -r, readable
# -w, writable
# -f, plain file 
# -d, directory
# -e tests if file exists 
if (-e $filename ) {
    
}

# Range Operator 
# numbers, letters, dates,
# The range operator returns a list and can be used to create arrays 
foreach my $i (1...10) {
    print "$i ":
}

# String concatenation operator 
say "str" . "ing"; 

## Quote operators 
say q(Hello World); ## no interpolation single quote marks
say qq() ## qq||, qq{} interprets variables like double quote 
say qw(one two three) ## returns a list 

# Operator Precedence and Associativity
# https://perldoc.perl.org/perlop.html#Operator-Precedence-and-Associativity
# similar to mathematics
```

## Context

```perl
# Perl supports two contexts list and scalar
my @arr = qw(one two three four five);
# an array can be used in list context or scalar context 
say foreach @array; # list context will iterate over each element. 
say scalar @array; # because we are accessing a scalar value it is the length of the arrays

# context is important as it affects how certain functions and structure behave in Perl. 

# A sub can determine what context it was called in by using `wantarray`.
if ( wantarray() ) {
   print "list\n";
}

# https://perlmaven.com/scalar-and-list-context-in-perl
```

## Regular Expressions

```perl
# string replacement
my $test = qq{I am a string with -- words and :: other characters};
$test =~ s/:/-/g;
say $test;

# string search
my $test = "this is a line"
my $re = qr/line/;
say $test =~ $re ? "True" : "False";

if ($test =~ /line/i) {
}
 
# extract a match 
my $match =~ /(line)/;

# extract list of matches
my $test = "this is a string";
my @match = $test =~ /i(.)/g;

# . 1 character
# + 1 or more 
# * 0 or more 
# whildcard matching is greedy use *? to prevent greedy
# (tex?t) ? # optionally match the "t"
# \s whitespace
# \S not whitespace
# \d digit 
# \D not digit 
# \w word class characters
# \W none word characters
# [1234asdlk] anything in the brackets
# [1-9] 1-9,  [^1-9] not 1-9
# meta characters  {} [] () ^ $ . | * + ? \
# use "\" to escape meta characters

# https://perldoc.perl.org/perlre.html
```

## Functions and Subroutines 

```perl
# Subroutines and Functions are essentially the same thing in Perl 
# Generally, it's best to return a scalar, a list, or a reference  
# IMPORTANT: The context a function is called in will propagate to the return value of the function.So the context of the return value will be either scalar or list based on how the context of the function. 
# A sub can determine what context it was called in by using `wantarray`.
if ( wantarray() ) {
   print "list\n";
}
# Function names have global scope and they can be called before they are defined in a script.
# Functions always return. Either explicitly by calling return or the last statement executed will be implicitly returned. It's recommended to always explicitly return
# Perl supports closures and higher order functions and is considered a functional programming language. 
# https://hop.perl.plover.com/
# http://www.perlmonks.org/?node_id=450922

hello();

sub hello {
	say "Hello";
}

say("Hello");

sub say {
	my $word = shift; # get variable from default array;
	say $word;
}

sub say {
	say foreach @_; # Get variables from default array 
}

# function references 
my $ref = \&func;

sub func {
    say "This is a func";
}

#either works
&{$ref}();
$ref->();

my $ref = sub { say "anonymously, yours"};
$ref->();

# reference an anonymous function  
my $ref = func();
$ref->();

sub func {
	# This is a closure in Perl.
	my $s = "I am a local variable";
	return sub { say $s };
}
```

## References 

```perl
# References are smaller pieces of memory that refer to larger pieces of memory. They are useful for working with arrays, hashes, and functions.

my @array = qw( one, two, three, four );
my $ref = \@array;
say foreach @{$ref}; # dereference the reference into the array it references

# create a reference to an anonymous array
my $ref = [qw( one, two, three, four )];

# parenthesis alone create an anonymous list 
$ref = ( one, two, three, four );
$ref->[0];

# create anonymous hash reference 
my $ref = {
    one => "one",
    two => "two"
};

# access element
${$ref}{one}
$ref->{one}

# function references 
my $ref = \&func;

sub func {
    say "This is a func";
}

# either works
&{$ref}();
$ref->();

my $ref = sub { say "anonymously, yours"};
$ref->();

# reference an anonymous function 
my $ref = func();
$ref->();

sub func {
	# this is a closure in Perl.
	my $s = "I am a local variable";
	return sub { say $s };
}

# finding a reference Type 
my $r = [one, two, three];
say ref($r);
# will return 'ARRAY'		
```

## File I/O 

```perl
# Perl reads files as a stream 

# < read
# + read and overwrite 
# >> append 

my $filename = "about.txt";
open (my $fh, '<', $filename ) or die "Can't open file: $!";
while (my $line = <$fh>) {
    chomp $line; # chomp is great for getting line endings correct for your OS 
    say $line;
}

close $fh;

# It's better to use a scoped variable than a bare word for the file handle
# since that defaults to global
# A bareword is a word without quotes that Perl allows to behave as a string.

# The IO file interface 
use 5.18.0;
use warnings;
use IO::File;

my $filename = 'lines.txt';
my $file = IO::File->new($filename, "r")

# Binary Files 
use 5.18.0;
use warnings;
use IO::File;

my $filename = 'pic.jpg';
my $copyfilename = 'copypic.jpg' 

my $file = IO::File->new($copyfilename, "r") or die "Cannot open output file $!";

# binmode is for Windows mostly, it doesn't hurt anything if not needed.
$file->binmode;
$copy->binmode;

my $buffer;
while (my $len = $file->read($buffer, 102400)) {
    $copy->print($buffer);
}

say "Done";
```

## Built In Functions 

https://perldoc.perl.org/index-functions-by-cat.html

```perl
# string functions 
say()   # say outputs a new line at the end of the output Perl 5.10+
print() # print and say default to standard stream for their output 

my @a = (1, 2, 3, 4);
my %h = (one => 1, two => 2, three => 3);

say join ', ', @a, %h;

say join ':', @a;

$s = "This is a string with lots of words in it.";
my @a = split /\s+/, $string;
say join ':', @a;

my $string = "This is a string";
say length $string;

say chomp $string. # removes line ending in string

say substr $string, 5, 7; # return a substring 
say substr($sring, 5, 7, 'too'); # replace

say index $string, 'is'; # returns index of first occurrence, 0 index.
say index $string, 'xis'; # returns -1 if not found
say index $string, 'is', 10; # start matching after 10 characters.
say rindex $string, 'is'; # match from right

say reverse($string); # reverse a string or a list;

say lc $string; # lowercase the string
say uc $string: # uppercase string
say ucfirst $string; # you get the idea

# numeric functions 
my $a = 4;
my $b = 12;
my $x = $a - $b;
my $x = abs $a - $b;
$x = sqrt $x;
$x = sqrt($a) ** 2;
$x = atan2(1,1) * 4;
$x = hex 'ff';
$x = oct '377';
$x = int $a /7 + .5; # returns integer portion 
my $x = rand; # rand b/t 0 and 1, first call to rand seeds the rand generator
my $x = rand 50;
my $x = int rand 50;
my $x = srand(); # seed the random number


# grep
my @a = qw( two four six eight );

say foreach grep /one/, @a; # match elements in the array
say foreach grep !/one/, @a; # don't match elements in the array
say foreach grep { !/one/ } @a; # don't match elements in the array

# map 
say foreach map { $_ * 9 } @a;

# time functions 
my $t = time(); # epoch time
my $timestring = localtime($t); #convert epoch to a list of time or a string,

# formatting time
# time in Perl is similar to the Unix C library 
my ($sec, $min, $hour, $mday, $mon, $year, $wday, $yday, $isdst ) = localtime($t);
$mon ++; # helps with 0 based 
$year += 1900; # the year needs to have this added since it's an orbital year

# add leading zeros to numerics for display 
foreach ($mon, $mday, $hour, $min, $sec) {
	$_ = "0$_" if $_ {number} = shift || 0;
    return $self;
}

sub number {
    my $self = shift;
    $self->{number} = shift if @_;
    return $self->{number} || 0;
}

sub version {
	shift;
	return $VERSION;
}

1; # for compatibility, end with a true value
```

## Best Practices 

```perl
# use ending semicolons 
# consistently format code blocks
# consistently name things
# Perl best practice is lowercase variable names

my $variable_name;
Package DL::Class;

my $object = DL::Class;

CONSTANT_NAME
# use constants wherever you would use them in another language 

use constant {
    TRUE => 1,
    FALSE => ""
};

use constant DEBUG => TRUE;

sub func_name { .... }

# use simple terse comments and whitespace 
# strict mode is on by default in Perl 5.18.0 + 
# use warnings is optional 
# you can use 'no warnings' inside of a sub routine 
# Try not to use the 'local' keyword. 
local #temp assigns a new value to a global variable and is a relic 
```

## Text Input

```perl
use feature 'say';

say "This is your last chance. What will it be, the red pill or the blue?";
say '...';
my $answer = readline;
chop $answer;
if ($answer =~ /blue/i) {
	say "The story is ends here for you.";
} elsif ($answer =~ /red/i) {
	say "You stay in Wonderland.";
} else {
	say "Agent Smith, you'll never win.";
}
```

## References

- Most of these notes are from Bill Weinman's course on Lynda.com 
- Bill Weinman [http://perl.bw.org](http://perl.bw.org)
- Lynda.com [https://www.lynda.com/Perl-tutorials/Perl-5-Essential-Training/447321-2.html](https://www.lynda.com/Perl-tutorials/Perl-5-Essential-Training/447321-2.html)
- [https://www.thegeekstuff.com/2010/01/20-killer-perl-programming-tips-for-beginners-on-unix-linux-os](https://www.thegeekstuff.com/2010/01/20-killer-perl-programming-tips-for-beginners-on-unix-linux-os)
- [https://perlmaven.com](https://perlmaven.com)
- [https://learn.perl.org/docs/keywords.html#perlvar](https://learn.perl.org/docs/keywords.html#perlvar)
- [https://stackoverflow.com/questions/6162484/why-does-modern-perl-avoid-utf-8-by-default](https://stackoverflow.com/questions/6162484/why-does-modern-perl-avoid-utf-8-by-default)
