FROM asciidoctor/docker-asciidoctor

# install Hugo
RUN apk update && \
    apk add --no-cache \
      hugo \
      go

# create asciidocter wrapper command for Hugo.
RUN echo $'#!/bin/sh\n\
/usr/bin/asciidoctor ${HUGO_AD_HTML_ROUGE_ARGS} \
-r asciidoctor-diagram -a imagesdir=\"static/${HUGO_AD_HTML_IMG_OUT_DIR}\" \
-r asciidoctor-mathematical -a mathematical-format=svg ${@} \
| sed -E -e "s@img src=\\"/?static/@img src=\\"${HUGO_AD_HTML_BASE_DIR}@g"\n\
' > /usr/local/bin/asciidoctor \
  && chmod +x /usr/local/bin/asciidoctor

# create asciidoctor-pdf wrapper command for User.
RUN echo $'Prawn::Svg::Font::GENERIC_CSS_FONT_MAPPING.merge!(\n\
  \'sans-serif\' => ENV[\'HUGO_AD_PDF_DEFAULT_FONT\']\n\
)\n\
' > /usr/local/lib/asciidoctor-pdf-cjk-config.rb \
  && chmod +x /usr/local/lib/asciidoctor-pdf-cjk-config.rb \
  && echo $'#!/bin/sh\n\
/usr/bin/asciidoctor-pdf -a imagesdir=${HUGO_AD_PDF_IMG_DIR} \
-a imagesoutdir=${HUGO_AD_PDF_IMG_OUT_DIR} \
-a scripts=cjk -r /usr/local/lib/asciidoctor-pdf-cjk-config \
-a pdf-theme=${HUGO_AD_PDF_THEME} \
-a source-highlighter=rouge -a rouge-style=${HUGO_AD_PDF_ROUGE_STYLE} \
-r asciidoctor-diagram \
-r asciidoctor-mathematical -a mathematical-format=svg ${@}\n\
' > /usr/local/bin/asciidoctor-pdf \
  && chmod +x /usr/local/bin/asciidoctor-pdf

# create hugo live preview wrapper command for User.
RUN echo $'#!/bin/sh\n\
/usr/bin/hugo server --watch --poll 700ms --disableFastRender\n' > /usr/local/bin/hugo-live \
  && chmod +x /usr/local/bin/hugo-live

# set environment variables
ENV HUGO_AD_HTML_ROUGE_ARGS="-a source-highlighter=rouge -a rouge-css=style -a rouge-style=molokai" \
    HUGO_AD_HTML_BASE_DIR="" \
    HUGO_AD_HTML_IMG_OUT_DIR="images/generated" \
    HUGO_AD_PDF_IMG_DIR="static/images" \
    HUGO_AD_PDF_IMG_OUT_DIR="static/images/generated" \
    HUGO_AD_PDF_THEME="default-with-fallback-font" \
    HUGO_AD_PDF_ROUGE_STYLE="molokai" \
    HUGO_AD_PDF_DEFAULT_FONT="M+ 1p Fallback"
