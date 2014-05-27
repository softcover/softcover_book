# Marketing and selling
\label{cha:marketing_selling}

Softcover combines the production system described starting in Chapter~\ref{cha:getting_started} with an easy-to-use sales platform. This chapter describes the steps supported by [Softcover.io](http://www.softcover.io/) to market and launch Softcover books and associated media.

## Screencasts and other media

Softcover is designed to make it easy to write and publish books, but books are only the foundation. Products like the [Ruby on Rails Tutorial](http://ruby.railstutorial.org/), [Learn Python the Hard Way](http://learnpythonthehardway.org/), and [The App Design Handbook](http://nathanbarry.com/app-design-handbook/) show the value of combining ebooks with other media (such as screencast videos) to create premium product bundles.

The current Softcover system supports automatic association of screencast ZIP files using a simple convention. To include a screencast product along with a book called `example_book`, simply create a screencast ZIP file `example_book.screencasts.zip` in the `media` directory. To include a preview screencast as well, create an m4v or mp4 formatted video file and put it in `media/preview.m4v`. <!-- Figure~\ref{fig:screencasts_dir} shows a sample screencasts directory with three screencasts, a zipped version of the three screencasts, and a preview. -->

<!-- ![Media directory structure.\label{fig:screencasts_dir}](images/figures/screencasts_dir.png) -->

Once the ZIP file is in place, you can publish the screencasts to Softcover with a simple command:

```console
$ softcover publish:media
```

\noindent To save time, this will only upload new files or ones that have changed since the last time you last published them.


## Marketing page

In addition to providing an online version of your book automatically, the Softcover sales platform includes a marketing page for online sales. The contents of the marketing page are determined by a configuration file, `marketing.yml`, in the `config` directory. The marketing file supports the use of limited Markdown (links, boldface, and emphasis/italics).


### Prices

The marketing file allows you to create up to three product bundles, each with different downloadable media formats, as seen in Listing~\ref{code:marketing_prices}.


\begin{codelisting}
\label{code:marketing_prices}
\codecaption{Pricing options and bundles. \\ \filepath{config/marketing.yml}}
```yaml
prices:
  -
    code: ebooks
    name: "HTML & Ebook"
    description:
      |
        * Optimized for computer screens, Kindle, and iPad
        * Incredibly awesome content
    media:
      - ebooks
    price: 3500

  -
    code: all
    name: "HTML, Ebook, and Screencasts"
    description:
      |
        * Includes 10 hours of video screencasts
        * Ebooks inclded in PDF/EPUB/MOBI formats
    media:
      - ebooks
      - media/screencasts
    price: 12500

```
\end{codelisting}

\noindent When published, the code in Listing~\ref{code:marketing_prices} produces the marketing page \linebreak shown in Figure~\ref{fig:pricing_options}. % Note that the price can optionally include a `regular_\-price` field, which displays a "regular" price with a strike-through (together with the actual price), as seen in the 3rd option in Figure~\ref{fig:pricing_options}.


![Pricing options.\label{fig:pricing_options}](images/figures/pricing_options.png)


### Author information and testimonials
\label{sec:author_information}

Softcover supports automatic display of author information (for single or multiple authors). Each author field should include a name, an image or Gravatar email, a contact email, and a brief biography (Listing~\ref{code:marketing_authors}). If you use `gravatar_email` instead of `image`, Softcover will automatically pull and display the [Gravatar](http://www.gravatar.com/) image associated with the given email address. (The Gravatar email itself won't be made public.)

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

Softcover also supports testimonials, each of which should include a name, title, image or Gravatar email, and text (Listing~\ref{code:marketing_testimonials}). As with author information, testimonials support either `image` or `gravatar_email`; if neither is defined, the Softcover logo will be used by default.

\begin{codelisting}
\label{code:marketing_testimonials}
\codecaption{Testimonials. \\ \filepath{config/marketing.yml}}
```yaml
testimonials:
  -
    name: "Person 1"
    title: "Person 1 Title"
    image: /images/testimonial_1.png
    text: "Testimonial 1 text" # limited markdown

  -
    name: "Person 2"
    title: "Person 2 Title"
    gravatar_email: "info@softcover.io"
    text: "Testimonial 2 text"

```
\end{codelisting}

The results of Listing~\ref{code:marketing_authors} and Listing~\ref{code:marketing_testimonials} appear in Figure~\ref{fig:authors_testimonials}.

![Authors and Testimonials.\label{fig:authors_testimonials}](images/figures/authors_testimonials.png)

### Frequently Asked Questions

Each Softcover marketing page includes an optional area for frequently asked questions (FAQs), as seen in Listing~\ref{code:marketing_faq}. Each FAQ has a question and an answer; the answer has limited Markdown support, while the question is plain text. Questions and answers appear on the website in the order defined.

\begin{codelisting}
\label{code:marketing_faq}
\codecaption{Frequently Asked Questions. \\ \filepath{config/marketing.yml}}
```yaml
faq:
  -
    question: "Question 1 text" # no markdown
    answer: "Answer 1 text" # limited markdown
  -
    question: "Question 2 text"
    answer: "Answer 2 text"

```
\end{codelisting}

### Additional information
\label{sec:additional_information}

For maximum flexibility, the Softcover marketing page includes a free-form "additional information" section (Listing~\ref{code:marketing_additional}). You can use this area to add any additional information about your book or pricing options. The contents of `marketing_content` will be rendered in full Markdown at the bottom of the page.

Another option, `contact_email`, gives readers a way to contact the author(s). In particular, the contents of the `contact_email` field provide users with a public `mailto` link to the given address.

Finally, Softcover includes an inconspicuous branded footer by default, but this can be overridden using `hide_custom_domain_footer`. Setting it to `true` disables the rendering of our branded Softcover footer on your custom domain (Section~\ref{sec:custom_domains}).

\begin{codelisting}
\label{code:marketing_additional}
\codecaption{Additional marketing information. \\ \filepath{config/marketing.yml}}
```yaml
marketing_content: ''
contact_email: ''
hide_custom_domain_footer: false
```
\end{codelisting}

## Site settings & customizations
\label{sec:site_settings}

### Custom domains
\label{sec:custom_domains}

In order to give authors maximum control, Softcover supports custom domains.  To use a custom domain for your book and other products, first create a `CNAME` record pointing to the following address:

```text
domains.softcover.io
```

\noindent For example, the full `CNAME` record might appear as follows:

```text
www.mynewbook.com CNAME domains.softcover.io
```

\noindent (*Note*: if you used "www" in the CNAME record, you must also use "www" in the custom domain field.) If you'd rather use the apex domain (without the "www"), you can use an `ALIAS` record if your DNS provider supports it:

```text
mynewbook.com ALIAS domains.softcover.io
```

Once you've added the `CNAME` (or `ALIAS`) record, add the full domain into the custom domain field on your book's manage page, as shown in Figure~\ref{fig:custom_domain_input}. With this setting, `www.mynewbook.com` will resolve automatically to the Softcover marketing page, and `www.mynewbook.com/book` will resolve to the HTML book page.

![Inputting the custom domain.\label{fig:custom_domain_input}](images/figures/custom_domain_field.png)


