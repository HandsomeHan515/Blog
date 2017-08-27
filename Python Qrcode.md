# Python QRcode

```
class JobCheckInView(APIView):

    def post(self, request):
        job = request.query_params['job']

        qr = qrcode.QRCode(
            version=1,
            error_correction=qrcode.constants.ERROR_CORRECT_L,
            box_size=10,
            border=4,
        )

        log.debug('data: {}'.format(data))

        qr.add_data(data)
        qr.make(fit=True)
        img = qr.make_image()
        # stream = BytesIO()
        from django.contrib.auth import settings
        media_filename = os.path.join(
            settings.MEDIA_ROOT, str(time.time()) + '.jpg')
        flolder = settings.MEDIA_URL
        img.save(media_filename, 'JPEG')
        num = random.randint(1000, 9999)

        data = {'img': media_filename, 'num': num}
        serializer = CheckSerializer(data, context={'request': request})

        return Response(serializer.data)
```
