ARG IMAGE
FROM ${IMAGE}

ARG HANDLER
ENV HANDLER=$HANDLER

COPY . .

RUN echo -n ${HANDLER} > /tmp/handler && \
    [ -z "${HANDLER}" ] && \
    ([ $(ls *.clj | wc -l) -eq "1" ] || (echo "Exactly 1 source file needed when HANDLER is empty" >&2 && exit 1)) && \
    (echo $(clojure -m func-server.read-ns $(ls *.clj))/handle > /tmp/handler || \
      (echo "Could not parse the namespace from source file $(ls *.clj)" >&2 && exit 1)) && \
    ([ $(clojure -i $(ls *.clj) -e "(fn? $(</tmp/handler))") == "true" ] || \
      (echo "Could not find 'handle' fn in source file $(ls *.clj)" >&2 && exit 1))

CMD clojure $([ -z "${HANDLER}" ] && echo "-i" "$(ls *.clj)") -m func-server.main $(</tmp/handler)
