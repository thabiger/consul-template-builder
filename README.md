# consul-template-builder

This repository configures CI/CD pipelines to build Consul Template with additional patches:

## CreateIndex attribute support

Having CreateIndex attribute available while rendering templates, allows to get the oldest (or the newest) instance of the service in following manner:

    {{- $backend := "" }}
    {{- $create_index := "18446744073709551615" | parseUint }}
    {{- range $c,$d:=service $name -}}
        {{- if lt $d.CreateIndex $create_index }}
            {{- $backend = (print "oldest_instance " $c " " $d.Address ":" $d.Port) }}
            {{- $create_index = $d.CreateIndex}}
        {{- end}}
    {{- end}}
    {{ $backend }}
