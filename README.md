# http-header �T�v

�T�u�f�B���N�g������ HTTP �T�[�o�փ��o�[�X�v���N�V������ɂ����� HTTP �w�b�_�ƂȂ�R���e�i�B

# �g����

## �f�t�H���g��

### ������

- /wiki �z���ւ̃A�N�Z�X�� `ryu310/pukiwiki-plus:7-apache-1.4.7-02` �� Reverse Proxy ����
- ���̑��A�N�Z�X�� Index �y�[�W��\������(�R���e���c�͋�)

### �C���X�g�[���ƋN��
```sh
$ git clone ��
$ docker-compose up -d
```

�C���X�g�[����A`http://localhost/` �ɃA�N�Z�X����� `Index of /` ���\�������B
�܂��A`http://localhost/wiki/` �ɃA�N�Z�X����� pukiwiki ���\�������B

## �J�X�^�}�C�Y��(Default �R���e���c��ǉ�)

## �J�X�^�}�C�Y��(Reverse Proxy ���ǉ�)

��: /wp �ɃA�N�Z�X����� Wordpress ��\������ꍇ�B

1. extra �f�B���N�g���� apache �̃R���t�B�O�t�@�C����ۊǂ���B
1. Dockerfile ��ҏW���ăR���t�B�O�t�@�C�����R���e�i�ɃR�s�[����B
```text
  : <snip>
# �t�@�C���ŉ����ɉ��L�̍s��ǉ�����
ProxyPass /wp http://wp/
ProxyPassReverse /wp http://wp/
```
1. docker-compose.yml ��ҏW���� wp ����IP�A�h���X�𖼑O�����ł���悤�ɂ���B
    - services �z���� wp �̐ݒ��ǉ�����
    - links �� wp ��ǉ�����
```
version: '2'
services:
  http_header:
    image: 'ryu310/http-header:2.4-01'
    build:
      context: .
      dockerfile: Dockerfile
    volumes:
      - 'http_header_config:/usr/local/apache2/conf'
    ports:
      - 80:80
    links:
      - wiki:wiki
      - wp:wp
    # Enable to restart container (It is useful for Docker for Windows)
    #restart: always
  wp:
    image: 'wordpress'
```
