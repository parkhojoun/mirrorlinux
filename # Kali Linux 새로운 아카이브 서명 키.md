# Kali Linux 새로운 아카이브 서명 키

![Kali Linux Signing Key](https://www.kali.org/blog/new-kali-archive-signing-key/images/new-kali-signing-key.jpg)

## 목차
- [요약](#요약)
- [상세 설명](#상세-설명)
- [해결 방법](#해결-방법)
- [새로운 시스템 구축](#새로운-시스템-구축)
- [자주 묻는 질문](#자주-묻는-질문)

## 요약

**Kali Linux 사용자들에게 나쁜 소식!** 앞으로 며칠 내에 거의 모든 사용자가 `apt update` 실행 시 다음과 같은 오류를 경험하게 됩니다:

```
Missing key 827C8569F2518CC677FECA1AED65462EC8D5E4C5, which is needed to verify signature.
```

이유는 Kali 저장소에 대한 새로운 서명 키를 만들어야 했기 때문입니다. 새 키를 수동으로 다운로드하고 설치해야 하며, 다음 명령어 한 줄로 해결할 수 있습니다:

```bash
sudo wget https://archive.kali.org/archive-keyring.gpg -O /usr/share/keyrings/kali-archive-keyring.gpg
```

이제 여러분의 Kali가 다시 롤링할 준비가 되었습니다! 불편을 끼쳐 드려 죄송합니다.

## 상세 설명

앞으로 며칠 내에 거의 모든 Kali 시스템에서 업데이트가 실패할 것입니다. `apt update` 실행 시 다음과 같은 오류 메시지를 보게 될 가능성이 높습니다:

```bash
┌──(kali㉿kali)-[~]
└─# sudo apt update
Get:1 https://http.kali.org/kali kali-rolling InRelease [41.5 kB]
Err:1 https://http.kali.org/kali kali-rolling InRelease
Sub-process /usr/bin/sqv returned an error code (1), error message is: Missing key 827C8569F2518CC677FECA1AED65462EC8D5E4C5, which is needed to verify signature.
```

이는 개별 사용자만의 문제가 아니라 모든 사용자에게 해당하는 문제이며, 전적으로 개발팀의 실수입니다. 저장소의 서명 키에 대한 액세스를 잃어버려서 새로운 키를 만들어야 했습니다. 동시에 저장소를 동결했기 때문에 (금요일 18일 이후 업데이트가 없었던 것을 눈치채셨을 것입니다) 아직 아무도 영향을 받지 않았습니다. 하지만 이번 주에 저장소 동결을 해제할 예정이며, 이제 새 키로 서명됩니다.

## 해결 방법

따라서 약간의 수동 작업이 필요합니다. 다음과 같이 새 키를 수동으로 다운로드하고 설치해야 합니다:

### wget 사용:
```bash
sudo wget https://archive.kali.org/archive-keyring.gpg -O /usr/share/keyrings/kali-archive-keyring.gpg
```

### curl 사용 (선호하는 경우):
```bash
sudo curl https://archive.kali.org/archive-keyring.gpg -o /usr/share/keyrings/kali-archive-keyring.gpg
```

### 체크섬 확인 (권장):
좋은 보안 관행으로, 파일의 체크섬이 아래와 일치하는지 확인해야 합니다:

```bash
┌──(kali㉿kali)-[~]
└─# sha1sum /usr/share/keyrings/kali-archive-keyring.gpg
603374c107a90a69d983dbcb4d31e0d6eedfc325 /usr/share/keyrings/kali-archive-keyring.gpg
```

### 새 키링 확인:
새 키링을 자세히 살펴볼 수도 있습니다. 이전 서명 키(`ED444FF07D8D0BF6`)와 새 서명 키(`ED65462EC8D5E4C5`)가 포함되어 있습니다:

```bash
┌──(kali㉿kali)-[~]
└─$ gpg --no-default-keyring --keyring /usr/share/keyrings/kali-archive-keyring.gpg -k
/usr/share/keyrings/kali-archive-keyring.gpg
--------------------------------------------
pub rsa4096 2025-04-17 [SC] [expires: 2028-04-17]
827C8569F2518CC677FECA1AED65462EC8D5E4C5
uid [ unknown] Kali Linux Archive Automatic Signing Key (2025) <devel@kali.org>
pub rsa4096 2012-03-05 [SC] [expires: 2027-02-04]
44C6513A8E4FB3D30875F758ED444FF07D8D0BF6
uid [ unknown] Kali Linux Repository <devel@kali.org>
sub rsa4096 2012-03-05 [E] [expires: 2027-02-04]
```

그리고 보시다시피, `apt update`가 여전히 작동합니다:

```bash
┌──(kali㉿kali)-[~]
└─# sudo apt update
[...]
68 packages can be upgraded. Run 'apt list --upgradable' to see them.
```

이제 시스템을 업데이트할 시간입니다!

## 새로운 시스템 구축

경우에 따라 Kali 시스템을 처음부터 다시 구축하는 것을 선호할 수도 있습니다. 이를 위해 새 키링이 포함된 모든 이미지를 업데이트했습니다.

[Get Kali](https://www.kali.org/get-kali/)로 이동하여 최신 이미지를 받으세요. 파일명의 버전이 `2025.1c`인 것을 확인할 수 있습니다. 이는 한 달 전에 릴리스한 것과 정확히 동일한 이미지이며, 유일한 차이점은 새 키링이 포함되어 있다는 것입니다. `2025-W17`부터 시작하는 주간 이미지도 사용할 수 있으며, 이들은 새 키링을 포함하고 있습니다.

또한 Kali NetHunter, VM, Cloud, Docker, WSL 등도 업데이트했습니다. 누락된 것이 있다고 생각되시면 알려주세요.

## 자주 묻는 질문

**Q. 키가 손상되었는데 인정하고 싶지 않은 것 아닌가요?**

A. 아닙니다. 보시다시피 키링에 여전히 이전 키를 포함하고 있습니다. 만약 손상되었다면 제거하고 취소 인증서를 제공했을 것입니다.

**Q. 이 새 키를 신뢰하지 않습니다! 정말 Kali Linux가 맞나요?**

A. 새 키는 Kali 팀의 일부 개발자들에 의해 서명되었으며, 서명은 Ubuntu OpenPGP 키서버에서 확인할 수 있습니다. [여기](https://keyserver.ubuntu.com/pks/lookup?search=827C8569F2518CC677FECA1AED65462EC8D5E4C5&fingerprint=on&op=index)에서 확인해보세요.

**Q. 잠깐, 기시감이 드는데요...**

A. 2018년에 실수로 GPG 키를 만료시킨 적이 있습니다... 이를 증명하는 [오래된 트윗](https://x.com/kalilinux/status/959515084157538304)이 여전히 남아있습니다.

## 추가 질문이나 지원

더 궁금한 점이 있으시거나 지원이 필요하시면 Kali Linux [포럼](https://forums.kali.org/), [Discord 채널](https://discord.kali.org/) 또는 [IRC 채널](https://www.kali.org/docs/community/kali-linux-irc-channel/) 중 원하시는 곳으로 연락하세요. 기꺼이 도와드리겠습니다.

---

> **참고**: 이 문서는 [Kali Linux 공식 블로그](https://www.kali.org/blog/new-kali-archive-signing-key/)의 내용을 마크다운으로 변환한 것입니다.
