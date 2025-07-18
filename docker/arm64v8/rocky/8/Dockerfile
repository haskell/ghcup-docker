FROM rockylinux:8

ENV LANG C.UTF-8

RUN dnf install --allowerasing -y \
            curl \
			gcc \
			gcc-c++ \
			binutils \
			coreutils \
			gmp-devel \
			ncurses-devel \
			libffi-devel \
			make \
			xz \
			tar \
			perl \
			bash \
			diffutils \
			git \
			gzip \
			gnupg \
			nodejs \
			llvm \
			clang && \
    dnf install --allowerasing -y \
            zlib-devel \
            bzip2-devel \
            openssl-devel \
            xz-devel

ARG GHCUP_VERSION=0.1.50.2
ARG GPG_KEY="7D1E8AFD1D4A16D71FADA2F2CCC85C0E40C06A8C FFEB7CE81E16A36B3E2DED6F2DE04D4E97DB64AD 88B57FCF7DB53B4DB3BFA4B1588764FBE22D19C4 EAF2A9A722C0C96F2B431CA511AAD8CEDEE0CAEF"


# install ghcup
RUN gpg --batch --keyserver keys.openpgp.org --recv-keys $GPG_KEY && \
    curl -sSfL -O https://downloads.haskell.org/~ghcup/$GHCUP_VERSION/aarch64-linux-ghcup-$GHCUP_VERSION && \
    curl -sSfL -O https://downloads.haskell.org/~ghcup/$GHCUP_VERSION/SHA256SUMS && \
    curl -sSfL -O https://downloads.haskell.org/~ghcup/$GHCUP_VERSION/SHA256SUMS.sig && \
    gpg --verify SHA256SUMS.sig SHA256SUMS && \
    sha256sum -c --ignore-missing SHA256SUMS && \
    mv aarch64-linux-ghcup-$GHCUP_VERSION /usr/bin/ghcup && \
    chmod +x /usr/bin/ghcup && \
    rm -rf SHA256SUMS SHA256SUMS.sig ~/.gnupg/public-keys.d/pubring.db.lock

ARG GHC=9.6.7
ARG CABAL_INSTALL=3.12.1.0

ENV GHCUP_CURL_OPTS="--silent"
ENV NO_COLOR=1

# install haskell toolchain
RUN ghcup config set gpg-setting GPGStrict && \
	ghcup --verbose install ghc   --isolate=/usr     --force ${GHC} && \
	ghcup --verbose install cabal --isolate=/usr/bin --force ${CABAL_INSTALL} && \
	find "/usr/lib/ghc-${GHC}/" \( -name "*_p.a" -o -name "*.p_hi" \) -type f -delete && \
	rm -rf "/usr/share/doc/ghc-${GHC}" && \
	rm -rf /tmp/ghcup* && \
	ghcup gc -p -s -c -t

ENV PATH /root/.cabal/bin:/root/.ghcup:/root/.local/bin:$PATH

