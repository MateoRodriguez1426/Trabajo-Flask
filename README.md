# Trabajo-Flask

    from email import message
    from flask import Flask
    from flask import request
    from flask import render_template

    app = Flask(__name__)


    shop = [{"name": "BuyPC", "item": [{"name": "keyboard", "price": 20}]}]

    @app.get("/")
    def index ():
        return render_template('index.html')

    @app.get("/shopping")
    def getshop ():
        return {"shop":shop}


    @app.post("/shopping")
    def createshop ():
        request_data = request.get_json()
        newshop = {"name": request_data["name"], "item": []}
        shop.append(newshop)
        return newshop, 201

    @app.post("/shopping/<string:name>/item")
    def create_item(name):
        request_data = request.get_json()
        for store in shop:
            if store["name"] == name:
                new_item = {"name": request_data["name"], "price": request_data["price"]}
                store["item"].append(new_item)
                return new_item, 201
        return {"message": "Store not found"}, 404


    @app.get("/shopping/<string:name>")
    def getshop2(name):
        for i in shop:
            if i["name"] == name:
                return i

      return {"message":"La tienda no ha sido encontrada"}, 404

    @app.get("/shopping/<string:name>/item")
    def get_item_in_store(name):
        for store in shop:
            if store["name"] == name:
                return {"item": store["item"]}
        return {"message": "Store not found"}, 404


    if __name__ == '__main__':
        app.run( debug = True, port= 7000)
