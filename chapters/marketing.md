# Marketing, launching, and selling
\label{cha:marketing_launch}

Softcover combines the production system described starting in Chapter~\ref{cha:getting_started} with an easy-to-use sales platform. This chapter describes the steps supported by Softcover.io to market and launch Softcover books and associated media.

## Publishing screencasts and other media

Softcover is designed to make it easy to write and publish books, but books are only the foundation. Experience with products like the [Ruby on Rails Tutorial](http://ruby.railstutorial.org/), [Learn Python the Hard Way](http://learnpythonthehardway.org/), and [The App Design Handbook](http://nathanbarry.com/app-design-handbook/) shows the value in combining ebooks with other media such as screencast videos to create premium product bundles.

The current Softcover system supports automatic association of screencast ZIP files using a simple convention. To include a screencast product along with a book called `example_book`, simply create a ZIP file with the right name in the `screencasts` directory, namely

```text
screencasts/example_book.screencasts.zip
```

\noindent To include a preview screencast as well, create an m4v or mp4 formatted video file and put it in `screencasts/preview.m4v`. A sample screencasts directory with three screencasts, a zipped version of same, and a preview, appears in Figure~\ref{fig:screencasts_dir}.

![Screencasts directory structure.\label{fig:screencasts_dir}](images/figures/screencasts_dir.png)

Then to publish your screencasts to Softcover, use `publish:screencasts`:

```console
$ softcover publish:screencasts
```

\noindent To save time, this will only upload new files or those that have changed since the last time you uploaded the files.


## Marketing page

In addition to providing an online version of your book automatically, the Softcover sales platform supplies a marketing page for online sales. The contents of the marketing page are determined by a configuration file, `marketing.yml`, in the `config` directory. The marketing file supports the use of limited Markdown (links, boldface, and emphasis/italics).


### Prices

The marketing file allows you to create up to three product bundles, each with different downloadable media formats (Listing~\ref{code:marketing_prices}).


\begin{codelisting}
\label{code:marketing_prices}
\codecaption{Pricing options and bundles. \\ \filepath{config/marketing.yml}}
```yaml
prices:
  -
    code: ebooks # unique identifier (shouldn't be changed)
    name: "HTML & Ebook" # appears as title of the option
    description: # limited Markdown
      |
        Optimized for Kindle and iPad
        Almost 9000 pages of content
        Includes a free copy of 1st Edition PDF
    media: # which downloadable media formats are included in this bundle
      - html
      - pdf
      - epub
      - mobi
    price: 4500 # the price in cents
  -
    code: screencasts
    name: "HTML & Screencasts"
    description:
      |
        More than 15 hours of hands-on Rails instruction
        Fully updated for the 2nd edition (Ruby 1.9/Rails 3.2)
        Includes two Rails 4.0 supplementary screencasts
        100% DRM-free digital downloads
        Includes copies of the 1st and 2nd edition screencasts and the
        Rails 4.0 supplement
    media:
      - html
      - screencasts
    price: 15900

  -
    code: all
    name: "HTML, Ebook, and Screencasts"
    description:
      |
        Get the full Rails Tutorial ebook/screencast bundle for the same
        price as the screencasts alone!
        Includes the full 2nd edition screencasts
        Includes two Rails 4.0 supplementary screencasts and the full
        Rails 4.0–compatible version of the book
        Three versions of the book with more than 600 pages each, and
        over 15 hours of video
        Recommended for new customers who learn well from screencasts
    media:
      - html
      - pdf
      - epub
      - mobi
      - screencasts
    price: 14900
    regular_price: 15900

```
\end{codelisting}


When published, the code in Listing~\ref{code:marketing_prices} produces the marketing page shown in Figure~\ref{fig:pricing_options}. Note that the price can optionally include a `regular_price` field, which displays a "regular" price with a strike-through (together with the actual price_), as seen in the 3rd option in Figure~\ref{fig:pricing_options}.


![Pricing options.\label{fig:pricing_options}](images/figures/pricing_options.png)


### Author information
\label{sec:author_information}

Softcover supports automatic display of author information (for one or for multiple authors). Each author field should include a name, image or Gravatar email, contact email, and brief biography (Listing~\ref{code:marketing_authors}). If you use `gravatar_email` instead of `image`, Softcover will automatically pull and display the associated [Gravatar](http://www.gravatar.com/) image. (The Gravatar email itself won't be made public.)

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

### Testimonials

Softcover also supports testimonials, each of which should include a name, title, image or Gravatar email, and text (Listing~\ref{code:marketing_testimonials}). As with author information (Section~\ref{sec:author_information}), testimonials support either `image` or `gravatar_email`; if neither is defined, the Softcover logo will be used by default. The results appear in Figure~\ref{fig:author_information}.

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


![Authors and Testimonials.\label{fig:authors_testimonials}](images/figures/authors_testimonials.png)

### Frequently Asked Questions

Each Softcover marketing page includes an optional area for frequently asked questions (FAQs) (Listing~\ref{code:marketing_faq}). Questions and answers appear on the website in the order defined. Each FAQ has a question and an answer; the answer text has limited Markdown support, while the question text is static.

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

For maximum flexibility, the Softcover marketing page includes a free-form "additional information" section (Listing~\ref{code:marketing_additional}). You can use this area to add any additional information about your book or pricing options. The contents of `marketing_content` will be rendered in full Markdown at the bottom of the page.

Another option, `contact_email`, gives readers a way to contact the author(s).In particular, the contents of the `contact_email` field provide users with a public `mailto` link to the given address.

Finally, Softcover includes an inconspicuous branded footer by default, but this can be overridden using `hide_custom_domain_footer`. The value of `hide_custom_domain_footer` is false by default, but setting it to true disables rendering of our branded Softcover footer on your custom domain.

\begin{codelisting}
\label{code:marketing_additional}
\codecaption{Additional marketing information. \\ \filepath{config/marketing.yml}}
```yaml
marketing_content: ''
contact_email: ''
hide_custom_domain_footer: false
```
\end{codelisting}

## Custom domains

In order to give authors maximum control, Softcover supports custom domains.  To use a custom domain for your book and other products, first create a `CNAME`record pointing to the following address:

```text
domains.softcover.io
```

\noindent For example, the full `CNAME` record might appear as follows:

```text
www.mynewbook.com CNAME domains.softcover.io
```

\noindent (Note: if you used "www" in the CNAME record, you must also use "www" in the custom domain field.) If you'd rather use the apex domain (without the "www"), you can use an `ALIAS` record if your DNS provider supports it:

```text
mynewbook.com ALIAS domains.softcover.io
```

Once you've added the `CNAME` (or `ALIAS`) record, add the full domain into the custom domain field on your book's manage page, as shown in Figure~\ref{fig:custom_domain_input}. With this setting, `www.mynewbook.com` will resolve automatically to the Softcover marketing page, and `www.mynewbook.com/book` will resolve to the HTML book page.

![Inputting the custom domain.\label{fig:custom_domain_input}](images/figures/custom_domain_field.png)

