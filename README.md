# apm for Boring Enterprise Nerds

[Boring Enterprise Nerds](https://boringenterprisenerds.com) want to show Markdown in ABAP

apm is a package manager 📦 for ABAP applications and modules, a website 🌐, and a registry 📑

![ben](https://raw.githubusercontent.com/mbtools/boring-enterprise-nerds/main/ben_system_banner.jpg)

```abap
*&---------------------------------------------------------------------*
*& Report Z_BEN_MARKDOWN
*&---------------------------------------------------------------------*
*& Boring Enterprise Nerds want to show Markdown in ABAP
*&---------------------------------------------------------------------*
REPORT z_ben_markdown.

PARAMETERS p_url TYPE string LOWER CASE OBLIGATORY
  DEFAULT `https://raw.githubusercontent.com/mbtools/boring-enterprise-nerds/refs/heads/main/README.md`.

START-OF-SELECTION.

  TRY.
      DATA(url) = /apmg/cl_url=>parse( p_url ).

      DATA(agent) = /apmg/cl_http_agent=>create( ).

      DATA(response) = agent->request( url = p_url ).

      IF response->is_ok( ) = abap_false.
        RAISE EXCEPTION TYPE /apmg/cx_error_text EXPORTING text = |HTTP Error { response->error( ) }|.
      ENDIF.

      DATA(markdown) = response->cdata( ).

      DATA(markdown_as_html) = NEW /apmg/cl_markdown( )->text( markdown ).

      DATA(html) = /apmg/cl_html=>create( ).

      html->add( '<html>' ).
      html->add( '<head>' ).
      html->add( '<title>Hello, Nerds!</title>' ).
      html->add( '<style>' ).
      html->add( /apmg/cl_markdown=>styles( ) ).
      html->add( /apmg/cl_emoji=>create( )->get_emoji_css( ) ).
      html->add( '</style>' ).
      html->add( '</head>' ).
      html->add( '<body>' ).
      html->add( '<div class="markdown">' ).
      html->add( /apmg/cl_emoji=>create( )->format_emoji( markdown_as_html ) ).
      html->add( '</div>' ).
      html->add( '</body>' ).
      html->add( '</html>' ).

      DATA(out) = html->render( ).

      cl_abap_browser=>show_html( html_string = out ).

    CATCH /apmg/cx_error INTO DATA(error).
      MESSAGE error->get_text( ) TYPE 'E' DISPLAY LIKE 'S'.
  ENDTRY.
```

![toronto](https://raw.githubusercontent.com/mbtools/boring-enterprise-nerds/main/Toronto_Background.jpg)
