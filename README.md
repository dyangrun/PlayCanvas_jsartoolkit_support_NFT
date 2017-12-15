# PlayCanvas_jsartoolkit_support_NFT
This File is For PlayCanvas AR Use NFT
![Display](look.jpg?raw=true)
How To Use
1use this file Change PlayCanvas Project jsartoolkit.mini.js
2add attributes to ArMarker likethis
```
ArMarker.attributes.add('NFTMarker1', {
    type: 'asset',
    assetType: 'binary',
    title: 'NFT1',
    description: 'TestNFTMarker'
});
ArMarker.attributes.add('NFTMarker2', {
    type: 'asset',
    assetType: 'binary',
    title: 'NFT2',
    description: 'TestNFTMarker'
});
ArMarker.attributes.add('NFTMarker3', {
    type: 'asset',
    assetType: 'binary',
    title: 'NFT3',
    description: 'TestNFTMarker'
});
```
3add NFT function in your project
Like This
```
 this.app.on('trackinginitialized', function (arController) {
        if(self.NFTMarker1 && self.NFTMarker2 && self.NFTMarker3)
        {
            //  var Url = self.NFTMarker1.getFileUrl();
            // Url = self.NFTMarker2.getFileUrl();
            // Url = self.NFTMarker3.getFileUrl();
            //  Url = pc.path.getDirectory(Url);//.substr(0,Url.indexOf("."));
             arController.loadNFTMarkerEX(self.NFTMarker1.getFileUrl(),self.NFTMarker2.getFileUrl(),self.NFTMarker3.getFileUrl(), function (markerId) {
                self.markerId = markerId;
            });
        }
        else if (self.pattern) {
            var Url2 = self.pattern.getFileUrl();
            arController.loadMarker(Url2, function (markerId) {
                self.markerId = markerId;
            });
        }
        arController.addEventListener('getMarker', function (ev) {
            var data = ev.data;
            var type = data.type;
            var marker = data.marker;
            if ((self.pattern && type === artoolkit.PATTERN_MARKER && marker.idPatt === self.markerId) ||
                (!self.pattern && type === artoolkit.BARCODE_MARKER && marker.idMatrix === self.matrixId)) {
                // Set the marker entity position and rotation from ARToolkit
                self.markerMatrix.data.set(data.matrix);
                if (arController.orientation === 'portrait') {
                    self.finalMatrix.mul2(self.portraitRot, self.markerMatrix);
                } else {
                    self.finalMatrix.mul2(self.landscapeRot, self.markerMatrix);
                }
                entity.setPosition(self.finalMatrix.getTranslation());
                entity.setEulerAngles(self.finalMatrix.getEulerAngles());

                console.log(entity.getRotation() + "POS:" + entity.getPosition());
                if (self.width > 0)
                    entity.setLocalScale(1 / self.width, 1 / self.width, 1 / self.width);

                // Z points upwards from an ARToolkit marker so rotate it so Y is up
                entity.rotateLocal(90, 0, 0);

                self.lastSeen = Date.now();
                if (!self.active) {
                    self.showChildren();
                    self.active = true;
                }
            }
        });
        
        arController.addEventListener('getNFTMarker', function (ev) {
            var data = ev.data;
            var type = data.type;
            var marker = data.marker;
            if (self.NFTMarker1) {
                // Set the marker entity position and rotation from ARToolkit

                self.markerMatrix.data.set(data.matrix);
                if (arController.orientation === 'portrait') {
                    self.finalMatrix.mul2(self.portraitRot, self.markerMatrix);
                } else {
                    self.finalMatrix.mul2(self.landscapeRot, self.markerMatrix);
                }
                
                var v3 = self.finalMatrix.getTranslation();
                v3.x *= 0.01;
                v3.y *= 0.01;
                v3.z *= 0.01;
                entity.setPosition(v3);
                entity.setEulerAngles(self.finalMatrix.getEulerAngles());

                
                //console.log(entity.getRotation() + "POS:" + entity.getPosition());
                if (self.width > 0)
                    entity.setLocalScale(1 / self.width, 1 / self.width, 1 / self.width);

                // Z points upwards from an ARToolkit marker so rotate it so Y is up
                entity.rotateLocal(90, 0, 0);
                
                self.lastSeen = Date.now();
                if (!self.active) {
                    self.showChildren();
                    self.active = true;
                }
            }
        });
    });
```	
	
this is simple,you can changer it
3 by oder add file to arMarker
  marker1 = *.fset
  marker2 = *.iset
  marker3 = *.fset3
  
OK,you can use NFT in PlayCanvas  