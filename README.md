# ghb
My Hugo files

  {{ .Summary }}
  {{ if .Truncated }}
  <div class="read-more-link">
    <a href="{{ .RelPermalink }}">Read More…</a>
  </div>
  {{ end }}
