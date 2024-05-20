# NAME

Perl::Critic::Git - Bond git and Perl::Critic to blame the right people for violations.

# VERSION

Version 1.3.3

# SYNOPSIS

        use Perl::Critic::Git;
        my $git_critic = Perl::Critic::Git->new(
                file   => $file,
                level  => $critique_level,         # or undef to use default profile
        );

        my $violations = $git_critic->report_violations(
                author => $author,
                since  => $date,                   # to critique only recent changes
        );

        my $diff_violations = $git_critic->diff_violations(
                from => $from_commit,
                to   => $to_commit,
        );

# METHODS

## new()

Create a new Perl::Critic::Git object.

        my $git_critic = Perl::Critic::Git->new(
                file   => $file,
                level  => $critique_level,         # or undef to use default profile
        );

Parameters:

- 'file'

    Mandatory, the path to a file in a Git repository.

- 'level'

    Optional, to set a PerlCritic level. If it is not specified, the default
    PerlCritic profile for the system will be used. Either name or number can
    assigned from the table below:

        +---------------+-----------------+
        | Severity Name | Severity Number |
        +---------------+-----------------+
        | gentle        | 5               |
        | stern         | 4               |
        | harsh         | 3               |
        | cruel         | 2               |
        | brutal        | 1               |
        +---------------+-----------------+

## get\_authors()

Return an arrayref of all the authors found in git blame for the file analyzed.

        my $authors = $git_critic->get_authors();

## report\_violations()

Report the violations for a given Git author.

        my $violations = $git_critic->report_violations(
                author => $author,
                since  => $date,                   # to critique only recent changes
        );

Parameters:

- author (mandatory)

    The name of the author to search violations for.

- since (optional)

    A date (format YYYY-MM-DD) for which violations of the PBPs that are older will
    be ignored. This allows critiquing only recent changes, instead of forcing your
    author to fix an entire legacy file at once if only one line needs to be
    modified.

- use\_cache (default: 0)

    Use a cached version of `git diff` when available. See
    [Git::Repository::Plugin::Blame::Cache](https://metacpan.org/pod/Git%3A%3ARepository%3A%3APlugin%3A%3ABlame%3A%3ACache) for more information.

## force\_reanalyzing()

Force reanalyzing the file specified by the current object. This is useful
if the file has been modified since the Perl::Critic::Git object has been
created.

        $git_critic->force_reanalyzing();

# ACCESSORS

## get\_perlcritic\_violations()

Return an arrayref of all the Perl::Critic::Violation objects found by running
Perl::Critic on the file specified by the current object.

        my $perlcritic_violations = $git_critic->get_perlcritic_violations();

## get\_blame\_lines()

Return an arrayref of Git::Repository::Plugin::Blame::Line objects corresponding
to the lines in the file analyzed.

        my $blame_lines = $self->get_blame_lines();

## get\_blame\_line()

Return a Git::Repository::Plugin::Blame::Line object corresponding to the line
number passed as parameter.

        my $blame_line = $git_critic->get_blame_line( 5 );

# INTERNAL METHODS

## \_analyze\_file()

Run "git blame" and "PerlCritic" on the file specified by the current object
and caches the results to speed reports later.

        $git_critic->_analyze_file();

Arguments:

- use\_cache (default: 0)

    Use a cached version of `git diff` when available.

## \_critic()

Lazy accessor for Perl::Critic object

        my $repo = $self->_critic();

## \_git\_repo()

Lazy accessor for Git::Repository object

        my $repo = $self->_git_repo();

## \_is\_analyzed()

Return whether the file specified by the current object has already been
analyzed with "git blame" and "PerlCritic".

        my $is_analyzed = $git_critic->_is_analyzed();

## \_get\_file()

Return the path to the file to analyze for the current object.

        my $file = $git_critic->_get_file();

## \_get\_critique\_level()

Return the critique level selected when creating the current object.

        my $critique_level = $git_critic->_get_critique_level();

## diff\_violations()

Report the violations for diff between commits.

        my $violations = $git_critic->diff_violations(
                from => $from_commit,
                to   => $to_commit,
        );

Parameters:

- from (mandatory)

    Commit or branch from which we will analyze changes

- to (mandatory)

    Commit or branch to which we will analyze changes

# SEE ALSO

- [Perl::Critic](https://metacpan.org/pod/Perl%3A%3ACritic)

# BUGS

Please report any bugs or feature requests through the web interface at
[https://github.com/guillaumeaubert/Perl-Critic-Git/issues](https://github.com/guillaumeaubert/Perl-Critic-Git/issues).
I will be notified, and then you'll automatically be notified of progress on
your bug as I make changes.

# SUPPORT

You can find documentation for this module with the perldoc command.

        perldoc Perl::Critic::Git

You can also look for information at:

- GitHub (report bugs there)

    [https://github.com/guillaumeaubert/Perl-Critic-Git/issues](https://github.com/guillaumeaubert/Perl-Critic-Git/issues)

- AnnoCPAN: Annotated CPAN documentation

    [http://annocpan.org/dist/Perl-Critic-Git](http://annocpan.org/dist/Perl-Critic-Git)

- CPAN Ratings

    [http://cpanratings.perl.org/d/Perl-Critic-Git](http://cpanratings.perl.org/d/Perl-Critic-Git)

- MetaCPAN

    [https://metacpan.org/release/Perl-Critic-Git](https://metacpan.org/release/Perl-Critic-Git)

# AUTHOR

[Guillaume Aubert](https://metacpan.org/author/AUBERTG),
`<aubertg at cpan.org>`.

# ACKNOWLEDGEMENTS

I originally developed this project for ThinkGeek
([http://www.thinkgeek.com/](http://www.thinkgeek.com/)). Thanks for allowing me to open-source it!

# COPYRIGHT & LICENSE

Copyright 2012-2018 Guillaume Aubert.

This code is free software; you can redistribute it and/or modify it under the
same terms as Perl 5 itself.

This program is distributed in the hope that it will be useful, but WITHOUT ANY
WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A
PARTICULAR PURPOSE. See the LICENSE file for more details.

# POD ERRORS

Hey! **The above document had some coding errors, which are explained below:**

- Around line 538:

    You forgot a '=back' before '=head1'
