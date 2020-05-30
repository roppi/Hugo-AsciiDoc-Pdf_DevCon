#FROM roppi/rc_hugo  # If you want to use a pre-built image, uncomment this line and delete the following line.
FROM asciidoctor/docker-asciidoctor

# install git and Hugo
RUN apk update && \
    apk add --no-cache \
    git \
    openssh-client \
    hugo

# install Japanese Fonts
RUN gem install --no-document \
    asciidoctor-pdf-cjk-kai_gen_gothic \
  && asciidoctor-pdf-cjk-kai_gen_gothic-install

# create asciidocter wrapper command for Hugo.
RUN echo $'#!/bin/sh\n\
/usr/bin/asciidoctor ${HTML_ROUGE_ARGS} \
-r asciidoctor-diagram -a outdir=\"static/${HTML_IMG_OUT_DIR}\" \
-r asciidoctor-mathematical -a mathematical-format=svg ${@} \
| sed -E -e "s/img src=\\"((diag|stem)-[^/]+)\\"/img src=\\"\/${HTML_BASE_DIR}${HTML_IMG_OUT_DIR}\/\\1\\"/"\n\
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
-a source-highlighter=rouge -a rouge-style=${PDF_ROUGE_STYLE} \
-r /usr/local/lib/asciidoctor-pdf-cjk-config.rb \
-r asciidoctor-pdf-cjk-kai_gen_gothic -a pdf-style=${PDF_STYLE} \
-r asciidoctor-diagram \
-r asciidoctor-mathematical -a mathematical-format=svg ${@}\n\
' > /usr/local/bin/asciidoctor-pdf \
  && chmod +x /usr/local/bin/asciidoctor-pdf

# set environment variables
ENV HTML_ROUGE_ARGS="-a source-highlighter=rouge -a rouge-css=style -a rouge-style=molokai" \
    HTML_BASE_DIR="" \
    HTML_IMG_OUT_DIR="images" \
    PDF_IMG_OUT_DIR="static/images" \
    PDF_DEFAULT_FONT="KaiGen Gothic JP" \
    PDF_ROUGE_STYLE="molokai" \
    PDF_STYLE="KaiGenGothicJP-theme.yml"
