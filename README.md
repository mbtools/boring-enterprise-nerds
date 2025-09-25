# apm for Boring Enterprise Nerds

Boring Enterprise Nerds want to show Markdown in ABAP

```abap
REPORT z_ben_markdown.

PARAMETERS p_url TYPE string LOWER CASE.

START-OF-SELECTION.

  TRY.
      DATA(url) = /apmg/cl_url=>parse( p_url ).

      DATA(agent) = /apmg/cl_http_agent=>create( ).

      DATA(response) = agent->request( url = p_url ).

      IF response->is_ok( ) = abap_false.
        RAISE EXCEPTION TYPE /apmg/cx_error_text EXPORTING text = |HTTP Error { response->code( ) }|.
      ENDIF.



    CATCH /apmg/cx_error INTO DATA(error).
      MESSAGE error->get_text( ) TYPE 'E' DISPLAY LIKE 'I'.
      EXIT.
  ENDTRY.
```

![ben](https://raw.githubusercontent.com/mbtools/boring-enterprise-nerds/main/ben_system_banner.jpg)

![toronto](https://raw.githubusercontent.com/mbtools/boring-enterprise-nerds/main/Toronto_Background.jpg)
