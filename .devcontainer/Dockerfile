#FROM roppi/rc_hugo  # If you want to use a pre-built image, uncomment this line and delete the following line.
FROM asciidoctor/docker-asciidoctor

# install Hugo
RUN apk update && \
    apk add --no-cache \
      hugo

# create asciidocter wrapper command for Hugo.
RUN echo $'#!/bin/sh\n\
/usr/bin/asciidoctor ${HTML_ROUGE_ARGS} \
-r asciidoctor-diagram -a imagesdir=\"static/${HTML_IMG_OUT_DIR}\" \
-r asciidoctor-mathematical -a mathematical-format=svg ${@}\n\
' > /usr/local/bin/asciidoctor \
  && chmod +x /usr/local/bin/asciidoctor

# create asciidoctor-pdf wrapper command for User.
RUN echo $'Prawn::Svg::Font::GENERIC_CSS_FONT_MAPPING.merge!(\n\
  \'sans-serif\' => ENV[\'PDF_DEFAULT_FONT\']\n\
)\n\
' > /usr/local/lib/asciidoctor-pdf-cjk-config.rb \
  && chmod +x /usr/local/lib/asciidoctor-pdf-cjk-config.rb \
  && echo $'#!/bin/sh\n\
/usr/bin/asciidoctor-pdf -a imagesdir=${PDF_IMG_OUT_DIR} \
-a scripts=cjk -r /usr/local/lib/asciidoctor-pdf-cjk-config \
-a pdf-theme=${PDF_THEME} \
-a source-highlighter=rouge -a rouge-style=${PDF_ROUGE_STYLE} \
-r asciidoctor-diagram \
-r asciidoctor-mathematical -a mathematical-format=svg ${@}\n\
' > /usr/local/bin/asciidoctor-pdf \
  && chmod +x /usr/local/bin/asciidoctor-pdf

# set environment variables
ENV HTML_ROUGE_ARGS="-a source-highlighter=rouge -a rouge-css=style -a rouge-style=molokai" \
    HTML_BASE_DIR="" \
    HTML_IMG_OUT_DIR="images" \
    PDF_IMG_OUT_DIR="static/images" \
    PDF_THEME="default-with-fallback-font" \
    PDF_ROUGE_STYLE="molokai" \
    PDF_DEFAULT_FONT="M+ 1p Fallback"
