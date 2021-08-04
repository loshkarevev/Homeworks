## API Gateway
Задание выполнено на основании [этой](https://www.nginx.com/blog/choosing-the-right-api-gateway-pattern/?__cf_chl_captcha_tk__=ed4062235a4eed746839ffaa4456039c320f9dec-1624120905-0-AYcP8ha43drdyJNKyF-DIGvZ1DR9YkX22Cl4O1eqj6VlImek5TI0LGefDCYx2x8nlce-REfCRIUyoqCAkDCcqNbjEPJCtKlCWPqty1WO-_5wzjjqkhHOIIP4GOQc-WRiQC4-XSYDvk_4svL_UsxTXQXtnach30UaMfB5o7Gf1AWSQtMK4FuNSBTxryzUZ4JAFyVrZVzzUWvWihqdKzk9otDOoK_kSzkjOTisu3XqYuBH6SsaiivUTx2TE20_hYQk4_URyMlzH7RhybZzZQL_KZakNAx2Aa5rBJt-ps0ETa_PZsEOgBBOv2MFeQu0DzaSqiLqnmboU47i38a0dKZg23_sq2wz7ULK3JZTSKsj6fIO-PTA-k_yncNs7odQNoL-hbaUsWor_aJX3cTcoDvok11rxniKHSQtkTKaGKgmi9s4SNft7kq6u0fkANzo77qLQQ7RJuwEAb5QuRba8ulZPQADD8vVC16JyY3aRET9DskxQg2T-1n6MGHjOtvjrNjhUojPMI0VU4oFjU7YqCvJyFdW-r4b43R2D3S6uOeJVNZjn4DbU-lKshHet4jd45PjgA0C7fANadJ9bstolosruifQyN_rWlRJiytg-yRLpZpxkFKEhRuC3C6R9JVbivKhP1gSc2OkLvKH8Fa4wk6QwIr03PSzA78Dnsb4JSE5MhvqaK_0XYqb95ZWYUKaFF40Jw) статьи.

| Параметр\Тип | Централизованный шлюз | Двухуровневый шлюз | Микро шлюз | Шлюзы Per-Pod | Sidecar Gateways and Service Mesh |
|---|---|---|---|---|---|
| SSL/TLS termination | + | + | + | + | + |
| Authentication | + | + |  | + | + |
| Authorization | + | + |  |  | + |
| Request routing | + |  | + |  |  |
| Rate limiting | + |  | + | + |  |
| Request/response manipulation | + |  |  |  |  |
| Facade routing | + | + |  |  |  |
| Centralized logging |  | + |  |  | + |
| Tracing injection |  | + |  |  | + |
| Service discovery |  | + | + |  | + |
| Load balancing |  | + | + |  | + |
| Authentication per API |  |  | + |  |  |
| Tracing and metrics generation |  |  |  | + |  |
| Error handling |  |  |  | + |  |
| Inter‑service authentication |  |  |  |  | + |
