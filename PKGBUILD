# Maintainer: Luchesar V. ILIEV <luchesar%2eiliev%40gmail%2ecom>
# Contributor: James An <james@jamesan.ca>
# Contributor: grimsock <lord.grimsock at gmail dot com>
# Contributor: David Danier <david.danier@team23.de>

pkgname=solr6
_pkgname=solr
pkgdesc="Popular, blazing fast open source enterprise search platform from the Apache Lucene project"

pkgver=6.3.0
pkgrel=1

arch=('any')
url="http://lucene.apache.org/${_pkgname}/"
license=('Apache')

depends=(
    'java-environment'
)

provides=("${_pkgname}=${pkgver}")
conflicts=("${_pkgname}")

_aptjar=auto-phrase-tokenfilter-1.0.jar
source=(
    "http://archive.apache.org/dist/lucene/${_pkgname}/${pkgver}/${_pkgname}-${pkgver}.tgz"
    "${_pkgname}-cloud.service"
    "${_pkgname}-node.service"
    "${_aptjar}"
)
sha256sums=(
    '07692257575fe54ddb8a8f64e96d3d352f2f533aa91b5752be1869d2acf2f544'
    '492569f40a90923f483afd4392857ab8ddcf7d4662d5cd0782ffd02c7ef6ecf0'
    'f7011eba0fda0f1abba3846c68d241bc565799608178ea0d38850a79ec4bd165'
    'SKIP'
)
noextract=(
    "${_aptjar}"
)
install="${_pkgname}.install"

backup=(
    opt/"${_pkgname}"/bin/solr.in.sh
    opt/"${_pkgname}"/server/contexts/"${_pkgname}"-jetty-context.xml
    opt/"${_pkgname}"/server/etc/webdefault.xml
    opt/"${_pkgname}"/server/etc/jetty{,-{http,https,ssl}}.xml
    opt/"${_pkgname}"/server/modules/{http,https,server,ssl}.mod
    opt/"${_pkgname}"/server/resources/{jetty-logging,log4j}.properties
    opt/"${_pkgname}"/server/"${_pkgname}"/{"${_pkgname}".xml,zoo.cfg}
)

package() {
    cd "${srcdir}"

    msg2 "Installing original ${_pkgname} files"
    mkdir "${pkgdir}/opt"
    cp -a "${_pkgname}-${pkgver}" "${pkgdir}/opt/${_pkgname}"
    mkdir "${pkgdir}/opt/${_pkgname}/run"

    msg2 'Removing unnecessary files'
    rm "${pkgdir}/opt/${_pkgname}/bin"/*.cmd

    msg2 'Installing custom files'
    install -Dm644 "${_aptjar}" \
        "${pkgdir}/opt/${_pkgname}/server/${_pkgname}-webapp/webapp/WEB-INF/lib/${_aptjar}"
    mkdir -p "${pkgdir}/usr/lib/systemd/system"
    for _service in "${_pkgname}"{-cloud,-node}.service ; do
        install -Dm644 "${_service}" "${pkgdir}/usr/lib/systemd/system/${_service}"
    done

    msg2 'Symlinking and fixing file ownership'
    mkdir -p "${pkgdir}/usr/bin"
    ln -s "/opt/${_pkgname}/bin/${_pkgname}" "${pkgdir}/usr/bin/${_pkgname}"
    ln -s "bin/${_pkgname}.in.sh" "${pkgdir}/opt/${_pkgname}/"
    install -o 521 -g 521 -m 0755 -d "${pkgdir}/opt/${_pkgname}/server/logs"
    for _file in \
        "${pkgdir}/opt/${_pkgname}/run" \
        "${pkgdir}/opt/${_pkgname}/server/${_pkgname}"
    do
        chown 521:521 "${_file}"
    done
}

# vim: set ts=4 sts=4 sw=4 et:
