<script>
  import { MetaTags } from 'svelte-meta-tags';
</script>

<MetaTags
  title="Another | Svelte Meta Tags"
  robots="noindex,nofollow"
  description="Description Another"
  canonical="https://www.canonical.ie/another"
  languageAlternates={[
    {
      hrefLang: 'de-AT',
      href: 'https://www.canonical.ie/de'
    }
  ]}
  mobileAlternate={{
    media: 'only screen and (max-width: 640px)',
    href: 'https://m.canonical.ie'
  }}
  openGraph={{
    url: 'https://www.url.ie/another',
    title: 'Open Graph Title Another',
    description: 'Open Graph Description Another',
    images: [
      {
        url: 'https://www.test.ie/og-image-another-01.jpg',
        width: 850,
        height: 650,
        alt: 'Og Image Alt Another'
      },
      {
        url: 'https://www.test.ie/og-image-another-02.jpg',
        width: 950,
        height: 850,
        alt: 'Og Image Alt Another Second'
      }
    ],
    siteName: 'SiteName Another'
  }}
  additionalMetaTags={[
    {
      property: 'dc:creator',
      content: 'Jane Doe'
    },
    {
      name: 'application-name',
      content: 'Svelte-Meta-Tags'
    },
    {
      httpEquiv: 'x-ua-compatible',
      content: 'IE=edge; chrome=1'
    },
    {
      name: 'googlebot',
      content: 'noindex,nofollow'
    }
  ]}
/>

<h1>Another SEO</h1>
