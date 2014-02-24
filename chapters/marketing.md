# Marketing, launching, and selling
\label{cha:marketing_launch}

This chapter will describe the steps supported by Softcover to market and launch books and associated media. It is currently in preparation.

### Publishing screencasts and other media

Create a zip of your screencast files in your book directory here: `screencasts/{your-book-slug}.screencasts.zip`, i.e: `screencasts/my-book.screencasts.zip`

To add a preview, create an m4v or mp4 formatted video file as: `screencasts/preview.m4v`

![Screencasts directory structure.\label{fig:screencasts_dir}](images/figures/screencasts_dir.png)

Then to publish your screencast directory, issue this command (it will only upload new files or those that have changed since last time):

```console
$ softcover publish:screencasts
```

### Marketing page

The Softcover website comes with a marketing page. Set its contents using `marketing.yml` in the `config` directory. Can use limited Markdown: links, bolds, and emphasis only.

The price can optionally include a ```regular_price``` field, which will display with a strike-through denoting a sale price as seen in the 3rd option below.

#### Prices

You can create up to 3 bundles, each with different downloadable media formats.

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
        Includes copies of the 1st and 2nd edition screencasts and the Rails 4.0 supplement
    media:
      - html
      - screencasts
    price: 15900

  -
    code: all
    name: "HTML, Ebook, and Screencasts"
    description:
      |
        Get the full Rails Tutorial ebook/screencast bundle for the same price as the screencasts alone!
        Includes the full 2nd edition screencasts
        Includes two Rails 4.0 supplementary screencasts and the full Rails 4.0–compatible version of the book
        Three versions of the book with more than 600 pages each, and over 15 hours of video
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

When published, this will look like:

![Pricing options.\label{fig:pricing_options}](images/figures/pricing_options.png)

#### Authors

Multiple authors can be added here. Instead of an ```image``` you can use ```gravatar_email``` which will pull the associated gravatar image from that email (it won't be made public). The ```bio``` section allows for limitted markdown support.

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

Like the Authors section, you can use ```image``` or ```gravatar_email```, otherwise a standin image of our logo will be used. The ```text``` section also has limitted markdown support.

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

#### Frequently Asked Questions

Questions and answers will be listed out in the same order. The answer text has limitted markdown support, while the question text is static. This section is optional.

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

#### Additional information

```marketing_content``` will be rendered in full markdown at the bottom of the page. You can use this to add any additional information about your book or pricing options.

```contact_email``` will provide users with a public ```mailto``` link.

\begin{codelisting}
\label{code:marketing_additional}
\codecaption{Additional marketing information. \\ \filepath{config/marketing.yml}}
```yaml
marketing_content: ''
contact_email: ''
hide_custom_domain_footer: false
```
\end{codelisting}

### Custom domains

To use a custom domain for your book, first create a CNAME record pointing to:

```domains.softcover.io```

i.e:
```www.mynewbook.com CNAME domains.softcover.io```

Or, if you'd rather use the apex domain, you could use an ALIAS record if your DNS provider supports it:

```mynewbook.com ALIAS domains.softcover.io```

Then, from your book's manage page, add the full domain into the custom domain field.

![Inputting the custom domain.\label{fig:custom_domain_input}](images/figures/custom_domain_field.png)

Note: if you used "www" in the CNAME record, you must also use "www" in the custom domain field.
