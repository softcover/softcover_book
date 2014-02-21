# Marketing, launching, and selling
\label{cha:marketing_launch}

This chapter will describe the steps supported by Softcover to market and launch books and associated media. It is currently in preparation.

### Publishing screencasts and other media

```console
$ softcover publish:screencasts
```

### Marketing page
\label{sec:marketing_page}

The Softcover website comes with a marketing page. Set its contents using `marketing.yml` in the `config` directory. Can use limited Markdown.

#### Authors

\begin{codelisting}
\label{code:marketing_authors}
\codecaption{Author(s) information. \\ \filepath{config/marketing.yml}}
```yaml
authors:
  -
    name: "The Author"
    image: /images/testimonial_1.png
    contact_email: "info@softcover.io"
    bio:
      |
        Author bio [link](https://www.softcover.io)

```
\end{codelisting}

#### Testimonials

\begin{codelisting}
\label{code:marketing_testimonials}
\codecaption{Testimonials. \\ \filepath{config/marketing.yml}}
```yaml
testimonials:
  -
    name: "Person 1"
    title: "Person 1 Title"
    image: /images/testimonial_1.png
    text: "Testimonial 1 text"

  -
    name: "Person 2"
    title: "Person 2 Title"
    gravatar_email: "info@softcover.io"
    text: "Testimonial 2 text"

```
\end{codelisting}

#### Frequently Asked Questions

\begin{codelisting}
\label{code:marketing_faq}
\codecaption{Frequently Asked Questions. \\ \filepath{config/marketing.yml}}
```yaml
faq:
  -
    question: "Question 1 text"
    answer: "Answer 1 text"

  -
    question: "Question 2 text"
    answer: "Answer 2 text"
```
\end{codelisting}

#### Additional information

\begin{codelisting}
\label{code:marketing_additional}
\codecaption{Additional marketing information. \\ \filepath{config/marketing.yml}}
```yaml
contact_email: ''
hide_custom_domain_footer: false
```
\end{codelisting}

### Custom domains

Put in info about custom domains
