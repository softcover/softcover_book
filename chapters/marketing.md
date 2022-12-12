# Marketing and selling
\label{cha:marketing_selling}

Softcover combines the production system described starting in Chapter~\ref{cha:getting_started} with an easy-to-use sales platform. This chapter describes the steps supported by [Softcover.io](https://www.softcover.io/) to market and launch Softcover books and associated media.

We start with simple instructions for optionally including media bundles (Section~\ref{sec:screencasts_and_other_media}), and then describe the steps needed to create a marketing page for your products (Section~\ref{sec:marketing_page}). We then describe site settings and customizations (Section~\ref{sec:site_settings}), such as public access, free or pay HTML books, custom domains, and Google Analytics.

## Screencasts and other media
\label{sec:screencasts_and_other_media}

Softcover is designed to make it easy to write and publish books, but books are only the foundation. Products like the [Ruby on Rails Tutorial](https://www.railstutorial.org/), [Learn Python the Hard Way](http://learnpythonthehardway.org/), and [The App Design Handbook](http://nathanbarry.com/app-design-handbook/) show the value of combining ebooks with other media (such as screencast videos) to create premium product bundles.

The current Softcover system supports automatic association of screencast ZIP files using a simple convention. To include a screencast product along with a book called `example_book`, simply create a screencast ZIP file `example_book.screencasts.zip` in the `media` directory. To include a preview screencast as well, create an m4v or mp4 formatted video file and put it in `media/preview.m4v`.

Once the ZIP file is in place, you can publish the screencasts to Softcover with a simple command:

```console
$ softcover publish:media
```

\noindent To save time, this will only upload new files or ones that have changed since the last time you last published them.

## Book description
\label{sec:book_description}

The main description for the book should be placed after the `description:` key in `config/book.yml`.

## Marketing page
\label{sec:marketing_page}

In addition to providing an online version of your book automatically, the Softcover sales platform includes a marketing page for online sales. The contents of the marketing page are determined by a configuration file, `marketing.yml`, in the `config` directory. The marketing file supports the use of limited Markdown (links, boldface, and emphasis/italics).

To update the marketing page with the contents of the latest `marketing.\-yml` file, use the `publish` command:

```console
$ softcover publish
```


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
        * Ebooks inclded in PDF/-EPUB/-MOBI formats
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

## Site settings and customizations
\label{sec:site_settings}

In this section we'll discuss the various ways to customize your Softcover site's settings using the Manage page for your book. The examples are drawn from a real site hosted on Softcover, the [Ruby on Rails Tutorial](https://www.railstutorial.org/). For example, Figure~\ref{fig:manage_button} shows the button to the Manage page at www.railstutorial.org.

![The link to the Manage page.\label{fig:manage_button}](images/figures/manage_button.png)

### Access options

As seen in Figure~\ref{fig:access_options}, Softcover supports four principal access options: public access, able to purchase, HTML paywall, and free download.

![Setting access options.\label{fig:access_options}](images/figures/access_options.png)

1. **Public Access**: Set this to **ON** to make your site available to the public. Default is **OFF**.

2. **Able to Purchase**: Set this to **ON** to enable users to purchase your products. Default is **OFF**.

3. **HTML Paywall**: Set this to **ON** to put the HTML copy of your book behind a paywall, so that only paying customers will be able to read it. Default is **OFF**.

4. **Free Download**: Set this to **ON** to enable free downloads of the PDF/-EPUB/-MOBI formats of your ebook. Default is **OFF**.

The settings in Figure~\ref{fig:access_options} arrange for the Ruby on Rails Tutorial to be accessible to the public and available for purchase, while making the HTML (but not the ebooks) available for free. See Section~\ref{sec:launch_sequence} for some additional recommendations on how to use these options.

### Custom domains
\label{sec:custom_domains}

In order to give authors maximum control, Softcover supports custom domains.  To use a custom domain for your book and other products, please email `info@softcover.io` for custom support.

### Google Analytics

Softcover supports Google Analytics integration. Just put your site's tracking id in the relevant box (Figure~\ref{fig:google_analytics}) and the proper JavaScript snippet will automatically be included on your marketing and book pages.

![Enabling Google Analytics integration.\label{fig:google_analytics}](images/figures/google_analytics.png)

### Miscellaneous content

The final form field on the Manage page inserts miscellaneous content on your book's purchase page (Figure~\ref{fig:miscellaneous_content}). It was originally included to support a legacy affiliate program used by the Rails Tutorial called zferral (now updated as [Ambassador](http://getambassador.com/), which is the site new users should use). It uses [Handlebars](http://handlebarsjs.com/) syntax like

```text
rev={{amount}}&customerId={{email}}&uniqueId={{id}}&serviceId={{id}}
```

\noindent to include the purchase amount, customer email address, and unique purchase id on the purchase page. It is unlikely that you'll need this functionality unless you are also supporting an affiliate program or something similar.

![Including miscellaneous content on downloads page.\label{fig:miscellaneous_content}](images/figures/miscellaneous_content.png)

## A typical launch sequence
\label{sec:launch_sequence}

Using the Softcover features, a typical launch sequence might go like this:

1. Begin work on the book with public access **OFF**.
2. After finishing a few chapters, set up a custom domain, analytics, and a basic marketing page, then set Public Access **ON**. Collect email address and feedback. Reach out to bloggers in your niche to get feedback and possible links.
3. Continue publishing the book-in-progress (typically one chapter at a \linebreak time). Send announcements via email, Twitter, etc., when each chapter is out.
4. Start selling access to the ebook-in-progress by adding an ebook product category in `marketing.yml` and setting Able to Purchase to **ON**.
5. Announce book's completion while adding a product tier for access to add-on goods as they're produced.
6. Complete add-on products and announce via email, Twitter, etc. Continue reaching out to bloggers, etc., to get inbound links and SEO.
