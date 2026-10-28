FROM alpine
RUN apk add --no-cache curl
CMD ["sh", "-c", "if curl -fs -o /dev/null --max-time 5 http://backend:5000/health; then echo 'backend: OK'; else echo 'backend: UNREACHABLE'; fi"]