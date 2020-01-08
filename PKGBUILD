# Maintainer: Philip Müller <philm[at]manjaro[dog]org>
# Contributor: Fabian Bornschein <plusfabi[at]gmail[dog]com>

pkgname=('mhwd-amdgpu')
pkgver=19.1.0
pkgrel=1

pkgdesc="MHWD module-ids for amdgpu"
arch=('any')
url="http://xorg.freedesktop.org/"
license=('custom')
source=("https://raw.githubusercontent.com/pciutils/pciids/master/pci.ids")
md5sums=('82cb2772c4146fe38782895d95002009')

_IDFILE="amdgpu.ids"
_IDFILE_EXP="amdgpu_exp.ids"

prepare() {
    # Names of supported hardware generations
    AMD_DEVNAMES=("Bristol Ridge"
                   "Raven Ridge"
                   "Stoney Ridge"
                   "Fiji"
                   "Tonga"
                   "Topaz"
                   "Amethyst"
                   "Polaris"
                   "Polaris10"
                   "Polaris11"
                   "Polaris12"
                   "Baffin"
                   "Ellesmere"
                   "Lexa"
                   "Vega")

    AMD_DEVNAMES_EXP=("Chelsea"
                    "Cape Verde"
                    "Heathrow"
                    "Wimbledon"
                    "Pitcairn"
                    "Sun"
                    "Oland"
                    "Mars"
                    "Venus"
                    "Neptune"
                    "Jet Pro"
                    "Iceland"
                    "Opal"
                    "Exo Pro"
                    "Meso"
                    "Strato Pro"
                    "Bonaire"
                    "Saturn"
                    "Strato"
                    "Roland"
                    "Tropo"
                    "Hawaii"
                    "Vesuvius"
                    "Tahiti"
                    "New Zealand"
                    "Malta"
                    "Kaveri"
                    "Kabini"
                    "Temash"
                    "Beema"
                    "Mullins"
                    "Curacao"
                    "Grenada")


    # Check for file
    if [ ! -f "${srcdir}/pci.ids" ] || ! grep "List of PCI ID's" "${srcdir}/pci.ids" > /dev/null
        then
            echo "Missing or wrong source PCIID-file"
            exit 1
        fi

    ## Extract AMD part
    # Remove everything before ^1002
    sed -i -n '/^1002/,$p' "${srcdir}/pci.ids"
    # Remove everything below ^1003
    sed -i '/^1003/,$d' "${srcdir}/pci.ids"
    # Remove comments
    sed -i '/^#/ d' "${srcdir}/pci.ids"
    # Leading tab
    sed -i 's/^\t//g' "${srcdir}/pci.ids"
    # Remove sub-IDs
    sed -i '/^\t/ d' "${srcdir}/pci.ids"

    # Create the final output file
    echo -e "# List generated by mhwd. Do not edit manually." > "${srcdir}/${_IDFILE}"
    echo -e "# List generated by mhwd. Do not edit manually." > "${srcdir}/${_IDFILE_EXP}"

    # Loop this part for every device name of AMDGPU
    for (( i=0; i<${#AMD_DEVNAMES[@]}; i++ ));
    do
        # Ignore generation without entry
        if grep -wi " ${AMD_DEVNAMES[i]}" "${srcdir}/pci.ids" > /dev/null
            then
                echo "+${AMD_DEVNAMES[i]}"
                # Add all the ID's
                grep -wi " ${AMD_DEVNAMES[i]}" "${srcdir}/pci.ids" | grep -vi "audio" | grep -v "#" | cut -d" " -f1 >> "${srcdir}/${_IDFILE}"
        fi
    done

    # Loop again for AMDGPU experimental
    for (( i=0; i<${#AMD_DEVNAMES_EXP[@]}; i++ ));
    do
        # Ignore generation without entry
        if grep -wi " ${AMD_DEVNAMES_EXP[i]}" "${srcdir}/pci.ids" > /dev/null
            then
                echo "+${AMD_DEVNAMES_EXP[i]}"
                # Add all the ID's
                grep -wi " ${AMD_DEVNAMES_EXP[i]}" "${srcdir}/pci.ids" | grep -vi "audio" | grep -v "#" | cut -d" " -f1 >> "${srcdir}/${_IDFILE_EXP}"
        fi
    done
}

package() {
    msg "Create IDS folder"
    install -d -m755 "${pkgdir}/var/lib/mhwd/ids/pci/"
    msg "Install IDS files"
    install -D -m644 "${srcdir}/${_IDFILE}" \
    "${pkgdir}/var/lib/mhwd/ids/pci/${_IDFILE}"
    msg "Install IDS files (experimental)"
    install -D -m644 "${srcdir}/${_IDFILE_EXP}" \
    "${pkgdir}/var/lib/mhwd/ids/pci/${_IDFILE_EXP}"
}
